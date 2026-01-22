# Observation

## Definition

> *An Observation provides a value for an ObservedProperty of a Feature, as observed by a Sensor. This value can be of any type, as described by the resultType of the Datastream that Observation is associated with.* [^1]

> [^1]: Hylke van der Schaaf — **Open Geospatial Consortium (OGC)**,  
> **SensorThings API 2.0 (23-019)** *(DRAFT)*,  
> version 23-019.  
> https://hylkevds.github.io/23-019/23-019.html



## Result Compliance Based on `datastream.type`
Two BEFORE triggers (INSERT and UPDATE) apply expected structure and domain rules to the observation’s `result`, according to the type defined in the linked datastream row:
- `observation_bi_type_constraints` (BEFORE INSERT)
- `observation_bu_type_constraints` (BEFORE UPDATE OF result_*, iddatastream)

The rules by type are:

### Quantity
- Expected structure: only `result_real` NOT NULL; `result_text` and `result_boolean` must be NULL.
- Constraints with respect to the bounds (if configured in the datastream):
  - if `value_min` is not NULL → `result_real >= value_min`;
  - if `value_max` is not NULL → `result_real <= value_max`.

### Category
- Expected structure: only `result_text` NOT NULL; `result_real` and `result_boolean` must be NULL.
- Codelist membership: `result_text` must exist as `codelist.id` in the collection equal to `datastream.codespace` (join by collection).

### Boolean
- Expected structure: only `result_boolean` NOT NULL with value 0 or 1; `result_text` and `result_real` must be NULL.

### Count
- Expected structure: only `result_real` NOT NULL, but it must be integer in value (comparison `result_real = CAST(result_real AS INTEGER)`). `result_text` and `result_boolean` must be NULL.
- Constraints with respect to the bounds (if present in the datastream):
  - if `value_min` is not NULL → `result_real >= value_min`;
  - if `value_max` is not NULL → `result_real <= value_max`.

### Text
- Expected structure: only `result_text` NOT NULL; `result_real` and `result_boolean` must be NULL.

### Bounds Consistency Between Datastream and Observations (Related Logic)
Although they are not triggers “of the observation table,” two triggers on `datastream` ensure that any changes to `value_min`/`value_max` do not make existing observations inconsistent:
- `datastream_bu_bounds_validate_observations` (BEFORE UPDATE OF value_min, value_max) blocks the update if there exist observations (for that datastream) with `result_real` outside the new limits. Applies to types Quantity and Count.
- In addition, `datastream_bi/bu_bounds_consistency` prevent setting `value_min > value_max` when both are non-NULL.
- For Count, `datastream_bi/bu_count_bounds_are_integers` enforce that the extrema, if present, are integers in value (the “integer” form is verified with numeric cast, e.g., 3.0 is accepted).

This family therefore guarantees semantic stability: you cannot tighten a series’ admissibility interval in such a way as to “expel” measurements already acquired.

>[!IMPORTANT]
>### Error Messages and Diagnosis
>The triggers use `RAISE(ABORT, '...')` with specific messages (e.g., “Type Quantity: result_real is below value_min.”, “Type Category: result_text must exist in codelist...”), facilitating diagnosis during data ingestion and ETL automation.

## Alignment of the Datastream Time Window
To maintain in `datastream` a consistent summary of the temporal coverage of its observations, there are three triggers that recompute the fields `datastream.phenomenontime_start` (MIN) and `datastream.phenomenontime_end` (MAX) with respect to all related rows in `observation`:
- `observation_ai_recalc_ds_times` (AFTER INSERT): upon a new insert, recomputes MIN/MAX on the datastream of `NEW.iddatastream`.
- `observation_au_recalc_ds_times` (AFTER UPDATE OF phenomenontime_*, iddatastream): recomputes for the new datastream; if the observation has been moved across series, it also recomputes for the old datastream.
- `observation_ad_recalc_ds_times` (AFTER DELETE): upon deletion, recomputes MIN/MAX on the datastream of `OLD.iddatastream`.

To support these recurring aggregations, recommended indexes are defined:
- `idx_observation_ds_times (iddatastream, phenomenontime_start, phenomenontime_end)`
  
>[!WARNING]
> Records that do not comply with the defined constraints **shall be rejected by the system and shall not be persisted** in the GeoPackage.

<p>&nbsp;</p>

<p>
  <img src="../assets/observation.svg"
     alt="TABELLA SOILSITE EXP"
     align="left"
     width="420">
 <!--
  <strong>TABELLA SOILSITE EXP</strong><br>
   Lorem Ipsum.
 -->
</p>

<br clear="all">
<p>&nbsp;</p>

## Table: `observation`

### Columns

| Name | Type | Constraints | Description |
|------|------|-------------|-------------|
| `id` | `INTEGER` | PRIMARY KEY | A unique, read-only attribute that serves as an identifier for the entity. |
| `guid` | `TEXT` |  | Universally unique identifier. |
| `phenomenontime_start` | `DATETIME` | NOT NULL, DEFAULT strftime('%Y-%m-%dT%H:%M:%fZ', 'now') | The time instant when the Observation happens, or the start time if it happens over a period. |
| `phenomenontime_end` | `DATETIME` |  | The end time if the Observation happens over a period. |
| `result_text` | `TEXT` |  | The estimated value of an ObservedProperty from the Observation, when the Type is Category or Text. |
| `result_real` | `REAL` |  | The estimated value of an ObservedProperty from the Observation, when the Type is Quantity or Count. |
| `result_boolean` | `BOOLEAN` |  | The estimated value of an ObservedProperty from the Observation, when the Type is Boolean. |
| `resulttime` | `DATETIME` |  | The time of the Observation’s result was generated. |
| `validtime_start` | `DATETIME` |  | The start time of the period during which the result may be used. |
| `validtime_end` | `DATETIME` |  | The end time of the period during which the result may be used. |
| `properties` | `TEXT` |  | mime type: 'application/json'. A JSON Object containing user-annotated properties as key-value pairs. |
| `guid_datastream` | `TEXT` | NOT NULL | Foreign key to the Datstream table, guid field. |


### Table Identifiers
In this table, the primary key is the *id* field (integer, auto-incrementing).  
There is also a text field named **GUID**, which stores a *UUID* (Universally Unique Identifier) compliant with RFC 4122.

Although GUID is not mandatory at the schema level (it is not declared NOT NULL), its functional requirement is enforced by two triggers:
- **observationguid (INSERT)** trigger: if GUID is missing or empty, a correctly formatted UUID is generated and inserted automatically.
- **observationguidupdate (UPDATE)** trigger: prevents any modification of GUID after insertion, making it immutable (effectively behaving as a stable key).  

Any foreign keys (FK) from other tables reference this table’s GUID field rather than the id field, ensuring stable and interoperable references across datasets and database instances.

> [!NOTE]
> **GUID management** is handled by database triggers, which ensure their automatic generation at the time of record insertion, **without any user involvement**.


### Relationships (as child)
- `observation.guid_datastream` → `datastream.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
  - *Note:* delete on `datastream` cascades to `observation`.

### Referenced by (as parent)
- None

### Indexes

| Name | Unique | Columns | Origin | Partial |
|------|--------|---------|--------|---------|
| `sqlite_autoindex_observation_2` | Yes | `phenomenontime_start`, `guid_datastream` | `u` | No |
| `sqlite_autoindex_observation_1` | Yes | `guid` | `u` | No |

### Triggers
For every trigger you will find:

- **When it runs** (BEFORE/AFTER, INSERT/UPDATE/DELETE)
- **What it reads and compares** (columns, lookups in other tables)
- **What happens on success** (the statement proceeds, optional updates)
- **What happens on failure** (the exact error text raised)

 
## `observationguid`

**When it runs:** AFTER INSERT (per row)

**What it reads and compares:**  
- Checks `NEW.guid` of the just-inserted row.

**What happens on success:**  
- If `NEW.guid` is `NULL`, it generates a lowercase UUID (v4-like) and updates the inserted row’s `guid`.  
- If `NEW.guid` is already set, nothing else happens. The insert succeeds.

**What happens on failure:**  
- **No failure path** — the trigger only assigns a GUID when missing.  
[1](https://github.com/INSPIRE-MIF/technical-guidelines/blob/2022.2/data/so/dataspecification_so.adoc)

---

## `observationguidupdate`

**When it runs:** AFTER UPDATE OF `guid` (per row)

**What it reads and compares:**  
- Compares `NEW.guid` vs `OLD.guid`.

**What happens on success:**  
- If `NEW.guid = OLD.guid` (no change), the update proceeds.

**What happens on failure:**  
- If `NEW.guid <> OLD.guid`, aborts with:  
  **`Cannot update guid column.`**  
[1](https://github.com/INSPIRE-MIF/technical-guidelines/blob/2022.2/data/so/dataspecification_so.adoc)

---

## `observation_bi_type_constraints`

**When it runs:** BEFORE INSERT (per row)

**What it reads and compares:**  
- Reads `NEW.result_text`, `NEW.result_real`, `NEW.result_boolean`, `NEW.guid_datastream`.  
- Looks up in `datastream` (by `guid = NEW.guid_datastream`):  
  - `type` ∈ {`Quantity`, `Category`, `Boolean`, `Count`, `Text`}  
  - `value_min`, `value_max`, `codespace`  
- For `Category`, validates membership in `codelist` where `codelist.collection = datastream.codespace` and `codelist.id = NEW.result_text`.

**What happens on success:**  
- Insert proceeds if the **shape** and **bounds/membership** rules match the `datastream.type`.

**What happens on failure (exact messages):**  
- **Quantity (shape):**  
  `Type Quantity: result_real must be NOT NULL; result_text and result_boolean must be NULL.`  
- **Quantity (min bound):**  
  `Type Quantity: result_real is below value_min.`  
- **Quantity (max bound):**  
  `Type Quantity: result_real exceeds value_max.`  
- **Category (shape):**  
  `Type Category: result_text must be NOT NULL; result_real and result_boolean must be NULL.`  
- **Category (membership):**  
  `Type Category: result_text must exist in codelist.id for the collection equal to the datastream codespace.`  
- **Boolean (shape):**  
  `Type Boolean: result_boolean must be 0 or 1; result_text and result_real must be NULL.`  
- **Count (shape):**  
  `Type Count: result_real must be an integer; result_text and result_boolean must be NULL.`  
- **Count (min bound):**  
  `Type Count: result_real is below value_min.`  
- **Count (max bound):**  
  `Type Count: result_real exceeds value_max.`  
- **Text (shape):**  
  `Type Text: result_text must be NOT NULL; result_real and result_boolean must be NULL.`  
[1](https://github.com/INSPIRE-MIF/technical-guidelines/blob/2022.2/data/so/dataspecification_so.adoc)

---

## `observation_bu_type_constraints`

**When it runs:** BEFORE UPDATE OF `result_text`, `result_real`, `result_boolean`, `guid_datastream` (per row)

**What it reads and compares:**  
- Same as the *INSERT* variant above, but re‑applied on update.

**What happens on success:**  
- Update proceeds if the **shape** and **bounds/membership** rules match the (possibly new) `datastream.type`.

**What happens on failure (exact messages):**  
- Same messages as in `observation_bi_type_constraints`.  
[1](https://github.com/INSPIRE-MIF/technical-guidelines/blob/2022.2/data/so/dataspecification_so.adoc)

---

## `observation_bi_result_text_in_codespace`

**When it runs:** BEFORE INSERT (per row)

**What it reads and compares:**  
- Fires **only if** `NEW.result_text IS NOT NULL` **and** the linked datastream has `type = 'Category'`.  
- Verifies `NEW.result_text ∈ codelist.id` where `codelist.collection = datastream.codespace`.

**What happens on success:**  
- Insert proceeds (membership satisfied).

**What happens on failure:**  
- Aborts with:  
  **`Insert denied: result_text must exist in codelist.id within the collection equal to the linked datastream codespace (type=Category).`**  
[1](https://github.com/INSPIRE-MIF/technical-guidelines/blob/2022.2/data/so/dataspecification_so.adoc)

---

## `observation_bu_result_text_in_codespace`

**When it runs:** **BEFORE INSERT** (per row)  
> **Note:** Although named “_bu_” (suggesting an UPDATE hook), in the DDL it is declared as **BEFORE INSERT** again.

**What it reads and compares:**  
- Same condition and lookup as `observation_bi_result_text_in_codespace`.

**What happens on success:**  
- Insert proceeds.

**What happens on failure:**  
- Aborts with the same message:  
  **`Insert denied: result_text must exist in codelist.id within the collection equal to the linked datastream codespace (type=Category).`**  
[1](https://github.com/INSPIRE-MIF/technical-guidelines/blob/2022.2/data/so/dataspecification_so.adoc)

---

## `observation_ai_recalc_ds_times`

**When it runs:** AFTER INSERT (per row)

**What it reads and compares:**  
- Aggregates all `observation` rows for `NEW.guid_datastream`.

**What happens on success:**  
- Updates the parent `datastream`:
  - `phenomenontime_start = MIN(observation.phenomenontime_start)`  
  - `phenomenontime_end   = MAX(COALESCE(observation.phenomenontime_end, observation.phenomenontime_start))`  
- No error is raised.

**What happens on failure:**  
- No explicit failure path (normal DML errors aside).  
[1](https://github.com/INSPIRE-MIF/technical-guidelines/blob/2022.2/data/so/dataspecification_so.adoc)

---

## `observation_au_recalc_ds_times`

**When it runs:** AFTER UPDATE OF `phenomenontime_start`, `phenomenontime_end`, `guid_datastream` (per row)

**What it reads and compares:**  
- Recomputes the range for the **new** `guid_datastream`.  
- If the observation moved to a different datastream, also recomputes the **old** one (only when `OLD.guid_datastream <> NEW.guid_datastream`).

**What happens on success:**  
- Updates the affected `datastream`(s) time range as above.

**What happens on failure:**  
- No explicit failure path.  
[1](https://github.com/INSPIRE-MIF/technical-guidelines/blob/2022.2/data/so/dataspecification_so.adoc)

---

## `observation_ad_recalc_ds_times`

**When it runs:** AFTER DELETE (per row)

**What it reads and compares:**  
- Aggregates remaining `observation` rows for `OLD.guid_datastream`.

**What happens on success:**  
- Updates the parent `datastream` time range as above.

**What happens on failure:**  
- No explicit failure path.  
[1](https://github.com/INSPIRE-MIF/technical-guidelines/blob/2022.2/data/so/dataspecification_so.adoc)



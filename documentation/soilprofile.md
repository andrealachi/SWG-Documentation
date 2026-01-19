## Table: `soilprofile`

### Columns

| Name | Type | Constraints | Description |
|------|------|-------------|-------------|
| `id` | `INTEGER` | PRIMARY KEY | Primary Key of the Table. |
| `guid` | `TEXT` |  | Universally unique identifier. |
| `inspireid_localid` | `TEXT` |  | A local identifier, assigned by the data provider. The local identifier is unique within the namespace, that is no other spatial object carries the same unique identifier. |
| `inspireid_namespace` | `TEXT` |  | Namespace uniquely identifying the data source of the spatial object. |
| `inspireid_versionid` | `TEXT` |  | The identifier of the particular version of the spatial object, with a maximum length of 25 characters. If the specification of a spatial object type with an external object identifier includes life-cycle information, the version identifier is used to distinguish between the different versions of a spatial object. Within the set of all versions of a spatial object, the version identifier is unique. |
| `localidentifier` | `TEXT` |  | Unique identifier of the soil profile given by the data provider of the dataset. |
| `beginlifespanversion` | `DATETIME` | NOT NULL, DEFAULT strftime('%Y-%m-%dT%H:%M:%fZ', 'now') | Date and time at which this version of the spatial object was inserted or changed in the spatial data set. |
| `endlifespanversion` | `DATETIME` |  | Date and time at which this version of the spatial object was superseded or retired in the spatial data set. |
| `validfrom` | `DATETIME` | NOT NULL, DEFAULT strftime('%Y-%m-%dT%H:%M:%fZ', 'now') | The time when the phenomenon started to exist in the real world. |
| `validto` | `DATETIME` |  | The time from which the phenomenon no longer exists in the real world. |
| `isderived` | `BOOLEAN` | NOT NULL, DEFAULT 0 | Boolean value to indicate whether the record is of Derived or Observed type. |
| `wrbversion` | `TEXT` |  | Indicates the WRB classification version. |
| `wrbreferencesoilgroup` | `TEXT` |  | First level of classification of the World Reference Base for Soil Resources. |
| `isoriginalclassification` | `BOOLEAN` | NOT NULL, DEFAULT 1 | Boolean value to indicate whether the WRB classification system was the original classification system to describe the soil profile. |
| `location` | `TEXT` |  | Foreign key to the SoilPlot table, guid field. |

### Table Identifiers
In this table, the primary key is the *id* field (integer, auto-incrementing).  
There is also a text field named **GUID**, which stores a *UUID* (Universally Unique Identifier) compliant with RFC 4122.

Although GUID is not mandatory at the schema level (it is not declared NOT NULL), its functional requirement is enforced by two triggers:
- **soilprofileguid (INSERT)** trigger: if GUID is missing or empty, a correctly formatted UUID is generated and inserted automatically.
- **soilprofileguidupdate (UPDATE)** trigger: prevents any modification of GUID after insertion, making it immutable (effectively behaving as a stable key).  

Any foreign keys (FK) from other tables reference this table’s GUID field rather than the id field, ensuring stable and interoperable references across datasets and database instances.


### Relationships (as child)
- `soilprofile.location` → `soilplot.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
  - *Note:* delete on `soilplot` cascades to `soilprofile`.

### Referenced by (as parent)
- `datastream.guid_soilprofile` → `soilprofile.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
- `derivedprofilepresenceinsoilbody.guid_soilprofile` → `soilprofile.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
- `isbasedonobservedsoilprofile.guid_soilprofile` → `soilprofile.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
- `isderivedfrom.guid_related` → `soilprofile.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
- `isderivedfrom.guid_base` → `soilprofile.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
- `othersoilnametype.othersoilname` → `soilprofile.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
- `profileelement.ispartof` → `soilprofile.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
- `wrbqualifiergroup_profile.guid_soilprofile` → `soilprofile.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)

### Indexes

| Name | Unique | Columns | Origin | Partial |
|------|--------|---------|--------|---------|
| `sqlite_autoindex_soilprofile_2` | Yes | `location` | `u` | No |
| `sqlite_autoindex_soilprofile_1` | Yes | `guid` | `u` | No |

### Triggers
For every trigger you will find:

- **When it runs** (BEFORE/AFTER, INSERT/UPDATE/DELETE)
- **What it reads and compares** (columns, lookups in other tables)
- **What happens on success** (the statement proceeds, optional updates)
- **What happens on failure** (the exact error text raised)

 



#### `soilprofileguid` / `soilprofileguidupdate`
**When they run:** AFTER INSERT / AFTER UPDATE OF `guid`

**What they do:** Auto-generate a GUID at insert if missing, and prevent changing it afterwards.

**If the check passes:** Insert writes GUID; updates with unchanged GUID proceed.

**If the check fails:** On change attempt, abort with: `Cannot update guid column.`

#### `i_ceckvalidperiodsoilprofile` / `u_ceckvalidperiodsoilprofile`
**When they run:** BEFORE INSERT / BEFORE UPDATE

**What they do:** Ensure `validfrom` is **not after** `validto`.

**If the check passes:** Statement proceeds.

**If the check fails:** Aborts with: `Table soilprofile: validto must be less than validfrom`.

#### `i_ceckvalidversionsoilprofile`
**When it runs:** BEFORE INSERT

**What it does:** Checks that `beginlifespanversion < endlifespanversion` (strict inequality).

**If the check passes:** Insert proceeds.

**If the check fails:** Aborts with: `Table soilprofile: beginlifespanversion must be less than endlifespanversion`.

#### `i_ceckprofileLocation` / `u_ceckprofileLocation`
**When they run:** BEFORE INSERT / BEFORE UPDATE

**What they do:** For **derived** profiles (`isderived = 1`), require `location` to be **NULL**.

**If the check passes:** Statement proceeds.

**If the check fails:** Aborts with: `Table soilprofile: For DERIVED profile (isderived = 1), location must be NULL`.

#### `i_ceckprofileLocationobserved` / `u_ceckprofileLocationobserved`
**When they run:** BEFORE INSERT (both, despite the `u_` prefix)

**What they do:** For **observed** profiles (`isderived = 0`), require `location` to be **NOT NULL**.

**If the check passes:** Insert proceeds.

**If the check fails:** Aborts with: `Table soilprofile: For OBSERVED profile (isderived = 0), location must be NOT NULL`.

#### `i_wrbreferencesoilgroup` / `u_wrbreferencesoilgroup`
**When they run:** BEFORE INSERT / BEFORE UPDATE

**What they do:** If `wrbversion` is set, check that `wrbreferencesoilgroup` exists in the **matching year’s** codelist collection (current/2014/2022).

**If the check passes:** Statement proceeds.

**If the check fails:** Aborts with (verbatim): `Table soilprofile: Invalid value for wrbversion. Must be present in id of the correct year codelist collection.`

#### `u_begin_today_soilprofile` / `u_begin_today_soilprofile_error`
**When they run:** AFTER UPDATE

**What they do:** Refresh `beginlifespanversion` to **now** when the record is still active; block edits when `endlifespanversion` is **past**.

**If the check passes:** Timestamp refreshed / update proceeds.

**If the check fails:** Aborts with: `If you change record endlifespanversion must be greater than today`.

#### `i_wrbproversion` / `u_wrbproversion`
**When they run:** BEFORE INSERT / BEFORE UPDATE

**What they do:** Require `wrbversion` to exist in `codelist.id` for `collection = 'wrbversion'`.

**If the check passes:** Statement proceeds.

**If the check fails:** Aborts with (verbatim): `Table soilprofile: Invalid value for wrbversion. Must be present in id of wrbreferencesoilgroupvalue codelist.`

---
<a id="soilsite"></a>
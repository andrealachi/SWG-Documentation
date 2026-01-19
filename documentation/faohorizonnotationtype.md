## Table: `faohorizonnotationtype`

### Columns

| Name | Type | Constraints | Description |
|------|------|-------------|-------------|
| `id` | `INTEGER` | PRIMARY KEY | Primary Key of the Table. |
| `guid` | `TEXT` |  | Universally unique identifier. |
| `faohorizondiscontinuity` | `INTEGER` |  | Number used to indicate a discontinuity in the horizon notation. |
| `faohorizonmaster_1` | `TEXT` | NOT NULL | First Symbol of the master part of the horizon notation. |
| `faohorizonmaster_2` | `TEXT` |  | Second Symbol of the master part of the horizon notation. |
| `faohorizonsubordinate_1` | `TEXT` |  | First Designations of subordinate distinctions and features within the master horizons and layers are based on profile characteristics observable in the field and are applied during the description of the soil at the site. |
| `faohorizonsubordinate_2` | `TEXT` |  | Second Designations of subordinate distinctions and features within the master horizons and layers are based on profile characteristics observable in the field and are applied during the description of the soil at the site. |
| `faohorizonsubordinate_3` | `TEXT` |  | Third Designations of subordinate distinctions and features within the master horizons and layers are based on profile characteristics observable in the field and are applied during the description of the soil at the site. |
| `faohorizonvertical` | `INTEGER` |  |  |
| `faoprime` | `TEXT` | NOT NULL | A prime and double prime may be used to connotate master horizon symbol of the lower of two respectively three horizons having identical Arabic-numeral prefixes and letter combinations. |
| `isoriginalclassification` | `BOOLEAN` | NOT NULL, DEFAULT 0 | Boolean value to indicate whether the FAO horizon notation was the original notation to describe the horizon. |
| `guid_profileelement` | `TEXT` |  | Foreign key to the ProfileElement table,  guid field. |


### Table Identifiers
In this table, the primary key is the *id* field (integer, auto-incrementing).  
There is also a text field named **GUID**, which stores a *UUID* (Universally Unique Identifier) compliant with RFC 4122.

Although GUID is not mandatory at the schema level (it is not declared NOT NULL), its functional requirement is enforced by two triggers:
- **faohorizonnotationtypeguid (INSERT)** trigger: if GUID is missing or empty, a correctly formatted UUID is generated and inserted automatically.
- **faohorizonnotationtypeguidupdate (UPDATE)** trigger: prevents any modification of GUID after insertion, making it immutable (effectively behaving as a stable key).  

Any foreign keys (FK) from other tables reference this table’s GUID field rather than the id field, ensuring stable and interoperable references across datasets and database instances.


### Relationships (as child)
- `faohorizonnotationtype.guid_profileelement` → `profileelement.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
  - *Note:* delete on `profileelement` cascades to `faohorizonnotationtype`.

### Referenced by (as parent)
- None

### Indexes

| Name | Unique | Columns | Origin | Partial |
|------|--------|---------|--------|---------|
| `sqlite_autoindex_faohorizonnotationtype_2` | Yes | `guid_profileelement` | `u` | No |
| `sqlite_autoindex_faohorizonnotationtype_1` | Yes | `guid` | `u` | No |

### Triggers
For every trigger you will find:

- **When it runs** (BEFORE/AFTER, INSERT/UPDATE/DELETE)
- **What it reads and compares** (columns, lookups in other tables)
- **What happens on success** (the statement proceeds, optional updates)
- **What happens on failure** (the exact error text raised)

 


#### `faohorizonnotationtypeguid` / `faohorizonnotationtypeguidupdate`
**When they run:** AFTER INSERT / AFTER UPDATE OF `guid`

**What they do:** Assign a GUID on insert when missing; prevent changing it later.

**If the check passes:** Insert writes GUID; unchanged updates proceed.

**If the check fails:** On change, abort with: `Cannot update guid column.`

#### `i_checkfaoprofileelementtype` / `u_ceckfaoprofileelementtype`
**When they run:** BEFORE INSERT / BEFORE UPDATE

**What they do:** Ensure the linked `profileelement` (`guid_profileelement`) is **HORIZON** (`profileelementtype = 0`).

**If the check passes:** Statement proceeds.

**If the check fails:** Aborts with: `Table faohorizonnotationtype: The associated profileelement must have profileelementtype = 0 (HORIZON)`.

#### FAO codelist validations (`i_`/`u_` variants)
- `faohorizonmaster_1`, `faohorizonmaster_2` must exist in **FAOHorizonMasterValue**.
- `faohorizonsubordinate_1/2/3` (when provided and in sequence) must exist in **FAOHorizonSubordinateValue**.
- `faoprime` must exist in **FAOPrimeValue**.

**If the check passes:** Statement proceeds.

**If the check fails:** The corresponding invalid-value message is raised and the statement aborts.

---
<a id="isbasedonobservedsoilprofile"></a>
## Table: `othersoilnametype`

### Columns

| Name | Type | Constraints | Description |
|------|------|-------------|-------------|
| `id` | `INTEGER` | PRIMARY KEY | Primary Key of the Table. |
| `guid` | `TEXT` |  | Universally unique identifier. |
| `othersoilname_type` | `TEXT` | NOT NULL | Name of the soil profile according to a specific classification scheme. |
| `othersoilname_class` | `TEXT` |  | Specific classification scheme. |
| `isoriginalclassification` | `BOOLEAN` | NOT NULL, DEFAULT 0 | Boolean value to indicate whether the specified classification scheme was the original classification scheme to describe the profile. |
| `othersoilname` | `TEXT` |  | Foreign key to the SoilProfile table, guid field. |

### Table Identifiers
In this table, the primary key is the *id* field (integer, auto-incrementing).  
There is also a text field named **GUID**, which stores a *UUID* (Universally Unique Identifier) compliant with RFC 4122.

Although GUID is not mandatory at the schema level (it is not declared NOT NULL), its functional requirement is enforced by two triggers:
- **othersoilnametypeguid (INSERT)** trigger: if GUID is missing or empty, a correctly formatted UUID is generated and inserted automatically.
- **othersoilnametypeguidupdate (UPDATE)** trigger: prevents any modification of GUID after insertion, making it immutable (effectively behaving as a stable key).  

Any foreign keys (FK) from other tables reference this table’s GUID field rather than the id field, ensuring stable and interoperable references across datasets and database instances.


### Relationships (as child)
- `othersoilnametype.othersoilname` → `soilprofile.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
  - *Note:* delete on `soilprofile` cascades to `othersoilnametype`.

### Referenced by (as parent)
- None

### Indexes

| Name | Unique | Columns | Origin | Partial |
|------|--------|---------|--------|---------|
| `sqlite_autoindex_othersoilnametype_1` | Yes | `guid` | `u` | No |

### Triggers
For every trigger you will find:

- **When it runs** (BEFORE/AFTER, INSERT/UPDATE/DELETE)
- **What it reads and compares** (columns, lookups in other tables)
- **What happens on success** (the statement proceeds, optional updates)
- **What happens on failure** (the exact error text raised)

 



#### `othersoilnametypeguid` / `othersoilnametypeguidupdate`
**When they run:** AFTER INSERT / AFTER UPDATE OF `guid`

**What they do:** Auto-generate a GUID at insert if missing; prevent changing it later.

**If the check passes:** Insert writes GUID; unchanged updates proceed.

**If the check fails:** On GUID change attempt, abort with: `Cannot update guid column.`

#### `i_soilname` / `u_soilname`
**When they run:** BEFORE INSERT / BEFORE UPDATE

**What they do:** Validate that `othersoilname_type` exists in the `OtherSoilNameTypeValue` codelist.

**If the check passes:** Statement proceeds.

**If the check fails:** Aborts with: `Table othersoilnametype: Invalid value for othersoilname_type. Must be present in id of othersoilnametypevalue codelist.`
---
<a id="profileelement"></a>
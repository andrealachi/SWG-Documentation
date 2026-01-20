# Other Horizon - Profile Element

<p>&nbsp;</p>

<p>
  <img src="assets/otherhorizon_profileelement.svg"
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

## Table: `otherhorizon_profileelement`

### Columns

| Name | Type | Constraints | Description |
|------|------|-------------|-------------|
| `id` | `INTEGER` | PRIMARY KEY | Primary Key of the Table. |
| `guid_profileelement` | `TEXT` | NOT NULL | Foreign key to the ProfileElement table, guid field. |
| `guid_otherhorizonnotationtype` | `TEXT` | NOT NULL | Foreign key to the OtherhorizonNotationType table, guid field. |

### Relationships (as child)
- `otherhorizon_profileelement.guid_otherhorizonnotationtype` → `otherhorizonnotationtype.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
  - *Note:* delete on `otherhorizonnotationtype` cascades to `otherhorizon_profileelement`.
- `otherhorizon_profileelement.guid_profileelement` → `profileelement.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
  - *Note:* delete on `profileelement` cascades to `otherhorizon_profileelement`.

### Referenced by (as parent)
- None

### Indexes

| Name | Unique | Columns | Origin | Partial |
|------|--------|---------|--------|---------|
| `sqlite_autoindex_otherhorizon_profileelement_1` | Yes | `guid_profileelement`, `guid_otherhorizonnotationtype` | `u` | No |

### Triggers
For every trigger you will find:

- **When it runs** (BEFORE/AFTER, INSERT/UPDATE/DELETE)
- **What it reads and compares** (columns, lookups in other tables)
- **What happens on success** (the statement proceeds, optional updates)
- **What happens on failure** (the exact error text raised)

 


#### `i_ceckothprofileelementtype` / `u_ceckothprofileelementtype`
**When they run:** BEFORE INSERT / BEFORE UPDATE

**What they do:** Ensure the linked `profileelement` is **HORIZON** (`profileelementtype = 0`).

**If the check passes:** Statement proceeds.

**If the check fails:** Aborts with: `Table otherhorizon_profileelement: The associated profileelement must have profileelementtype = 0 (HORIZON)`.

---
<a id="otherhorizonnotationtype"></a>

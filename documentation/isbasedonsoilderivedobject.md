## Table: `isbasedonsoilderivedobject`

### Columns

| Name | Type | Constraints | Description |
|------|------|-------------|-------------|
| `id` | `INTEGER` | PRIMARY KEY | Primary Key of the Table. |
| `guid_base` | `TEXT` | NOT NULL | Foreign key to the SoilDerivedObject table, guid field. - Base SoilDerivedObject. |
| `guid_related` | `TEXT` | NOT NULL | Foreign key to the SoilDerivedObject table, guid field. - Derived SoilDerivedObject. |

### Relationships (as child)
- `isbasedonsoilderivedobject.guid_related` → `soilderivedobject.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
  - *Note:* delete on `soilderivedobject` cascades to `isbasedonsoilderivedobject`.
- `isbasedonsoilderivedobject.guid_base` → `soilderivedobject.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
  - *Note:* delete on `soilderivedobject` cascades to `isbasedonsoilderivedobject`.

### Referenced by (as parent)
- None

### Indexes

| Name | Unique | Columns | Origin | Partial |
|------|--------|---------|--------|---------|
| `sqlite_autoindex_isbasedonsoilderivedobject_1` | Yes | `guid_base`, `guid_related` | `u` | No |

### Triggers
- None

---
<a id="isderivedfrom"></a>
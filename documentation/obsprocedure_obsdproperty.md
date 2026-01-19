## Table: `obsprocedure_obsdproperty`

### Columns

| Name | Type | Constraints | Description |
|------|------|-------------|-------------|
| `id` | `INTEGER` | PRIMARY KEY | A unique, read-only attribute that serves as an identifier for the entity. |
| `guid_observingprocedure` | `TEXT` | NOT NULL | Foreign key to the  Procedure table, guid field. |
| `guid_observedproperty` | `TEXT` | NOT NULL | Foreign key to the Observed Property table, guid field. |

### Relationships (as child)
- `obsprocedure_obsdproperty.guid_observedproperty` → `observedproperty.guid` (**ON UPDATE** CASCADE, **ON DELETE** RESTRICT)
- `obsprocedure_obsdproperty.guid_observingprocedure` → `observingprocedure.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
  - *Note:* delete on `observingprocedure` cascades to `obsprocedure_obsdproperty`.

### Referenced by (as parent)
- None

### Indexes

| Name | Unique | Columns | Origin | Partial |
|------|--------|---------|--------|---------|
| `sqlite_autoindex_obsprocedure_obsdproperty_1` | Yes | `guid_observingprocedure`, `guid_observedproperty` | `u` | No |

### Triggers
- None

---
<a id="obsprocedure_sensor"></a>
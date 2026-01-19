## Table: `unitofmeasure`

### Columns

| Name | Type | Constraints | Description |
|------|------|-------------|-------------|
| `id` | `INTEGER` | PRIMARY KEY | A unique, read-only attribute that serves as an identifier for the entity. |
| `code` | `TEXT` |  | The Unified Code for Units of Measure is a code system intended to include all units of measures being contemporarily used in international science, engineering, and business. |
| `label` | `TEXT` |  | A word or phrase used to name or describe something, often to identify or classify it. |
| `symbol` | `TEXT` |  | Standardized, unambiguous string used to represent a unit of measure. Symbols are designed for machine-to-machine communication and ensure consistent interpretation of units across systems and applications. |
| `qudt_unit` | `TEXT` |  | Is an entity that represents a standardized measure of a physical quantity. |
| `conversionoffset` | `REAL` |  | Is the value added after applying the multiplier, used for units that do not start at zero relative to the reference unit. |
| `conversionmultiplier` | `REAL` |  | Is the scaling factor used to convert a value from one unit to its reference unit (typically the SI base unit for that dimension). |

### Relationships (as child)
- None

### Referenced by (as parent)
- `datastream.code_unitofmeasure` → `unitofmeasure.code` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)

### Indexes

| Name | Unique | Columns | Origin | Partial |
|------|--------|---------|--------|---------|
| `sqlite_autoindex_unitofmeasure_1` | Yes | `code` | `u` | No |

### Triggers
- None

---
<a id="wrbqualifiergroup_profile"></a>
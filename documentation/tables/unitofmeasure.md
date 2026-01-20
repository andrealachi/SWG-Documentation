# Unit Of Measure
The uom attribute contains the unit of measurement and is an object that must contain either a code attribute with the UCUM code of the unit, or a href attribute with a URI pointing to a unit definition. It may also contain the label and symbol attributes that can be displayed in user interfaces.
The unitOfMeasure is part of the SWE-Common structure description found in resultType. 

<p>&nbsp;</p>

<p>
  <img src="assets/unitofmeasure.svg"
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

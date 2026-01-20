# Observed Procedure - Sensor

<p>&nbsp;</p>

<p>
  <img src="../assets/obsprocedure_sensor.svg"
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

## Table: `obsprocedure_sensor`

### Columns

| Name | Type | Constraints | Description |
|------|------|-------------|-------------|
| `id` | `INTEGER` | PRIMARY KEY | A unique, read-only attribute that serves as an identifier for the entity. |
| `guid_observingprocedure` | `TEXT` | NOT NULL | Foreign key to the  Procedure table, guid field. |
| `guid_sensor` | `TEXT` | NOT NULL |  |

### Relationships (as child)
- `obsprocedure_sensor.guid_sensor` → `sensor.guid` (**ON UPDATE** CASCADE, **ON DELETE** RESTRICT)
- `obsprocedure_sensor.guid_observingprocedure` → `observingprocedure.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)
  - *Note:* delete on `observingprocedure` cascades to `obsprocedure_sensor`.

### Referenced by (as parent)
- None

### Indexes

| Name | Unique | Columns | Origin | Partial |
|------|--------|---------|--------|---------|
| `sqlite_autoindex_obsprocedure_sensor_1` | Yes | `guid_observingprocedure`, `guid_sensor` | `u` | No |

### Triggers
- None

---
<a id="otherhorizon_profileelement"></a>


# Editing Datastreams in QGIS (GeoPackage)

This guide explains how to edit **Datastream** records in QGIS when working with the INSPIRE Soil GeoPackage. A Datastream in this schema aligns with the OGC SensorThings API concept and links time‑series observations to their context (Thing, Sensor, ObservedProperty) and to an optional Feature of Interest (Soil Site / Soil Profile / Profile Element / Soil Derived Object). citeturn22search13turn22search18

---

## 1) Why Datastream editing is slightly different
Unlike most attribute‑only tables, **`datastream` includes an optional geometry** (`observedarea` as **POLYGON**, EPSG:3035). This makes editing slightly more complex because a Datastream can behave either as a feature layer (when a geometry is present) or simply as an attribute table (when geometry is `NULL`). Both cases are valid in the DDL. citeturn4search1

> **Tip**  
> If `observedarea` is `NULL`, QGIS treats the layer like a non‑spatial table; you can still manage all relations and attributes normally. If you need a footprint, digitize or paste a polygon in EPSG:3035. citeturn4search1

---

## 2) Core fields and required relations
A Datastream record must include:

- **`name`** and **`type`** (constrained to `Quantity`, `Category`, `Boolean`, `Count`, `Text`). citeturn4search1
- **Links to context**: **`guid_sensor`** and **`guid_observedproperty`** are mandatory FKs (aligning with SensorThings, which requires a Datastream to link to a Sensor and an ObservedProperty; linkage to a Thing is optional in this schema). citeturn22search18turn4search1

**Feature of Interest (exclusive one‑of)**  
At most **one** among `guid_soilsite`, `guid_soilprofile`, `guid_profileelement`, `guid_soilderivedobject` can be populated (CHECK constraint allowing 0 or 1). This models an optional FOI link specific to the soil domain. citeturn4search1

---

## 3) Type‑specific rules and value domains
**Type set & combinations (CHECK)**  
The `type` field controls which auxiliary columns are allowed:
- `Quantity`: requires a unit (`code_unitofmeasure`), **no `codespace`**, optional numeric bounds.
- `Category`: requires a `codespace` (URI identifying the codelist collection), **no unit**, **no bounds**.
- `Boolean` / `Text`: **no unit**, **no codespace**, **no bounds**.
- `Count`: **no unit**, **no codespace**; if bounds are provided they must be integers (enforced by triggers). citeturn4search1

**Bounds order (CHECK + triggers)**  
If both `value_min` and `value_max` are set, `value_min <= value_max`. Extra BEFORE triggers prevent saving invalid numeric intervals and ensure integer bounds for `Count`. citeturn4search1

**Codespace membership (triggers)**  
When `codespace` is not `NULL`, its value must exist in the `codelist` table under the `Category` collection; otherwise, inserts/updates are aborted. citeturn4search1

**Definition/Codespace URI hygiene (CHECK)**  
`definition` and `codespace` are validated to be well‑formed URIs (no whitespace, correct scheme and authority). citeturn4search1

---

## 4) Time coherence and observation coupling
**Phenomenon time window**  
Datastreams maintain their `phenomenontime_start` and `phenomenontime_end` **automatically**: AFTER triggers recompute them from the linked `observation` rows (MIN start, MAX end with null‑safe handling). You do not need to populate them manually. citeturn4search1

**Procedure–Property pairing**  
If `guid_observingprocedure` is set, the pair `(guid_observingprocedure, guid_observedproperty)` **must exist** in the `obsprocedure_obsdproperty` mapping; INSERT/UPDATE is rejected otherwise. citeturn4search1

**Bounds tightening protection**  
Updating `value_min` / `value_max` is blocked when **existing observations** of the Datastream would fall outside the new range. This preserves consistency between series metadata and stored data. citeturn4search1

---

## 5) QGIS editing workflow
1. **Open the Datastream form** from the parent context (e.g., Thing, Soil Site/Profile/Element, or directly from the layer list). The relation widget pre‑populates keys where configured. citeturn4search1
2. **Geometry handling**  
   - If you need a footprint, enable editing and digitize **`observedarea`** (POLYGON, EPSG:3035).  
   - If not applicable, leave geometry **`NULL`** and treat the record as attribute‑only. citeturn4search1
3. **Set `type`** and complete **unit / codespace / bounds** according to the rules above; invalid combinations are rejected by CHECKs and triggers. citeturn4search1
4. **Link the required entities**: pick **Sensor** and **ObservedProperty**; optionally link a **Thing** and one **Feature of Interest** (exclusive one‑of). citeturn22search18turn4search1
5. **Save edits**; then, add **Observations** through the child panel. Observation‑level triggers will enforce shape (e.g., `result_real` vs `result_text`) and category membership (for `Category` series). Datastream times are recalculated automatically. citeturn4search1

---

## 6) Common validation messages (what they mean)
- **“Type Count: value_min/value_max must be an integer”** — bounds must be numerically integral for `Count`. citeturn4search1
- **“Insert/Update denied: (guid_observingprocedure, guid_observedproperty) pair not registered”** — create the pair in `obsprocedure_obsdproperty` first. citeturn4search1
- **“Codespace invalid: not in Category codelist”** — `codespace` must be present in `codelist` where `collection='Category'`. citeturn4search1
- **“Bounds update rejected: some observations fall outside new range”** — adjust bounds or the affected data. citeturn4search1

---

## 7) Alignment with SensorThings
This schema follows the SensorThings notion that a Datastream represents a **homogeneous series** identified by its **ObservedProperty** as measured by a **Sensor**, optionally associated with a **Thing** and a spatial **observed area**. Relations and creation order in SensorThings emphasise creating Thing/Sensor/ObservedProperty before the Datastream; your GeoPackage enforces the essential links via FKs and triggers. citeturn22search13turn22search18

---

## See also
- [OGC SensorThings API 2.0 (draft)](citeturn22search13)
- [Creating Entities — FROST‑Server](citeturn22search18)
- Your GeoPackage DDL for Datastreams, Observations, and mappings (triggers and CHECKs). citeturn4search1


# GeoPackage – CHECK / Trigger Constraints

*All constraints extracted from `DDL_SO_16.sql`.*

## soilsite
- **CHECK**: `validfrom <= validto` (enforced via BEFORE INSERT/UPDATE triggers).
- **CHECK**: `beginlifespanversion <= endlifespanversion` (BEFORE INSERT).
- **Codelist**: `soilinvestigationpurpose ∈ codelist(id)` where `collection='SoilInvestigationPurposeValue'` (BEFORE INSERT/UPDATE).
- **GUID immutability**: `guid` auto-generated on INSERT; updates to `guid` aborted.
- **Versioning**: on UPDATE of business fields, `beginlifespanversion` set to current time if `endlifespanversion` is NULL or future; update aborted if `endlifespanversion` is past.

## soilplot
- **CHECK**: `beginlifespanversion <= endlifespanversion` (BEFORE INSERT).
- **Codelist**: `soilplottype ∈ codelist(id)` where `collection='SoilPlotTypeValue'` (BEFORE INSERT/UPDATE).
- **GUID immutability**: `guid` auto-generated on INSERT; updates to `guid` aborted.
- **Versioning**: on UPDATE, `beginlifespanversion` refreshed if `endlifespanversion` is NULL/future; update aborted if `endlifespanversion` is past.

## soilprofile
- **CHECK**: `validfrom <= validto` (BEFORE INSERT/UPDATE).
- **CHECK**: `beginlifespanversion < endlifespanversion` (BEFORE INSERT).
- **Observed/Derived location rule**: if `isderived=1` then `location` must be NULL; if `isderived=0` then `location` must be NOT NULL (BEFORE INSERT/UPDATE).
- **WRB version membership**: `wrbversion ∈ codelist(id)` where `collection='wrbversion'` when not NULL (BEFORE INSERT/UPDATE).
- **WRB RSG + version coherence**: if `wrbversion` targets a specific year, `wrbreferencesoilgroup` must belong to the corresponding year collection (BEFORE INSERT/UPDATE).
- **GUID immutability** and **versioning refresh** on UPDATE.

## othersoilnametype
- **Codelist**: `othersoilname_type ∈ codelist(id)` where `collection='OtherSoilNameTypeValue'` (BEFORE INSERT/UPDATE).
- **GUID immutability**.

## isderivedfrom
- **Type pairing**: `guid_base` must reference a *derived* profile; `guid_related` must reference an *observed* profile (BEFORE INSERT/UPDATE).

## soilbody
- **CHECK**: `beginlifespanversion <= endlifespanversion` (BEFORE INSERT).
- **GUID immutability** and **versioning refresh** on UPDATE with past/future end checks.

## soilbody_geom
- **GUID immutability**.

## derivedprofilepresenceinsoilbody
- **CHECK (cumulative)**: sum of `lowervalue` for the same `guid_soilbody` must not exceed 100 (BEFORE INSERT/UPDATE).
- **Type pairing**: `guid_soilprofile` must reference a *derived* profile (BEFORE INSERT/UPDATE).

## soilderivedobject
- **GUID immutability**.

## isbasedonobservedsoilprofile
- **Type pairing**: `guid_soilprofile` must reference an *observed* profile (BEFORE INSERT/UPDATE).

## isbasedonsoilbody
- *(no CHECK/trigger logic in this file beyond FK/UNIQUE constraints).* 

## isbasedonsoilderivedobject
- *(no CHECK/trigger logic in this file beyond FK/UNIQUE constraints).* 

## profileelement
- **CHECK (depth range)**: at least one of `profileelementdepthrange_uppervalue` / `..._lowervalue` must be NOT NULL (BEFORE INSERT/UPDATE).
- **CHECK (range order)**: `uppervalue < lowervalue` enforced via BEFORE INSERT/UPDATE triggers.
- **CHECK (version)**: `beginlifespanversion <= endlifespanversion` (BEFORE INSERT).
- **Geogenic consistency**: when `layertype` is not `geogenic`, geogenic attributes must be NULL (BEFORE INSERT/UPDATE).
- **Horizon vs Layer fields**: when `profileelementtype = HORIZON`, layer-specific attributes must be NULL (BEFORE INSERT/UPDATE).
- **Codelists**: membership checks for `layertype`, `layerrocktype`, `layergenesisprocess`, `layergenesisenviroment`, `layergenesisprocessstate` (BEFORE INSERT/UPDATE).
- **GUID immutability** and **versioning refresh** on UPDATE.

## faohorizonnotationtype
- **Profileelement compatibility**: associated profileelement must have `profileelementtype = HORIZON` (BEFORE INSERT/UPDATE).
- **Codelists**: membership checks for `faohorizonmaster_1`, optional `faohorizonmaster_2`, `faohorizonsubordinate_1..3` (conditional), and `faoprime` (BEFORE INSERT/UPDATE).
- **GUID immutability**.

## otherhorizonnotationtype
- **Codelist**: `horizonnotation ∈ codelist(id)` where `collection='OtherHorizonNotationTypeValue'` (BEFORE INSERT/UPDATE).
- **GUID immutability**.

## otherhorizon_profileelement
- **Profileelement compatibility**: associated profileelement must have `profileelementtype = HORIZON` (BEFORE INSERT/UPDATE).

## wrbqualifiergrouptype
- **Codelists**: membership checks for `qualifierplace`, `wrbqualifier`, and optional `wrbspecifier_1/2`, each against the correct year-specific collection selected by `wrbversion` (BEFORE INSERT/UPDATE).
- **WRB version membership**: `wrbversion ∈ codelist(id)` where `collection='wrbversion'` when not NULL (BEFORE INSERT/UPDATE).
- **Uniqueness guard**: composite uniqueness (version/place/qualifier/specifiers) enforced via BEFORE INSERT/UPDATE triggers.
- **Specifier coherence**: `wrbspecifier_1` required if `wrbspecifier_2` is provided, and both specifiers must differ (BEFORE INSERT/UPDATE).
- **GUID immutability**.

## wrbqualifiergroup_profile
- **Version alignment**: `wrbversion` on linked `soilprofile` and `wrbqualifiergrouptype` must match (BEFORE INSERT/UPDATE).
- **Qualifier position uniqueness**: `qualifierposition` unique within the same profile for each `qualifierplace` (BEFORE INSERT/UPDATE).

## datastream
- **Type set**: `type ∈ {Quantity, Category, Boolean, Count, Text}` (CHECK).
- **Type combinations**: exclusive requirements among `code_unitofmeasure`, `codespace`, `value_min`, `value_max` by `type` (CHECK).
- **Bounds order**: if both bounds set, `value_min <= value_max` (CHECK + BEFORE INSERT/UPDATE).
- **Count bounds as integers**: when `type='Count'`, provided bounds must be numerically integral (BEFORE INSERT/UPDATE).
- **Codespace membership**: if `codespace` is not NULL, it must exist in `codelist(id)` for `collection='Category'` (BEFORE INSERT/UPDATE).
- **Procedure–Property pairing**: `(guid_observingprocedure, guid_observedproperty)` must exist in `obsprocedure_obsdproperty` on INSERT/UPDATE (BEFORE triggers).
- **Definition/Codespace URI hygiene**: syntax checks on `definition` and `codespace` (CHECK).
- **Phenomenon time propagation**: AFTER triggers keep `phenomenontime_*` equal to MIN/MAX across linked observations.
- **Bounds tightening protection**: updating `value_min/max` is aborted if any existing observation would fall outside the new bounds (BEFORE UPDATE).

## observation
- **Valid time order**: `validtime_end > validtime_start` when both provided (CHECK).
- **Type-shape enforcement**: BEFORE INSERT/UPDATE triggers validate `result_*` per parent datastream `type`, including NULLability, boolean domain, integer requirement for `Count`, and compliance with `value_min/max` where defined.
- **Category membership**: when parent datastream type is `Category`, `result_text` must exist in `codelist(id)` for that datastream `codespace` (BEFORE INSERT/UPDATE).
- **Datastream time window maintenance**: AFTER triggers recalculate the parent datastream `phenomenontime_*` after INSERT/UPDATE/DELETE.

## observedproperty
- **Definition URI hygiene**: syntax checks via CHECK constraints.
- **GUID immutability**.

## sensor
- **Metadata/encoding rule**: if `metadata` is NOT NULL then `encodingtype ∈ {'PDF','HTML'}`; otherwise both NULL (CHECK).
- **Definition URI hygiene**; **GUID immutability**.

## thing
- **Definition URI hygiene** via CHECK; **GUID immutability**.

## observingprocedure
- **Definition URI hygiene** via CHECK; **GUID immutability**.

## obsprocedure_obsdproperty
- *(no additional CHECK/trigger logic beyond the UNIQUE pair; pairing is validated when referenced by datastream triggers).* 

## obsprocedure_sensor
- *(no additional CHECK/trigger logic beyond the UNIQUE pair).* 

## unitofmeasure
- *(no CHECK/trigger logic in this file).* 

## codelist
- *(no CHECK/trigger logic in this file).* 

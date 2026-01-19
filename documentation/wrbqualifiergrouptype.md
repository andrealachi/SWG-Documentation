## Table: `wrbqualifiergrouptype`

### Columns

| Name | Type | Constraints | Description |
|------|------|-------------|-------------|
| `id` | `INTEGER` | PRIMARY KEY | Primary Key of the Table. |
| `guid` | `TEXT` |  | Universally unique identifier. |
| `wrbversion` | `TEXT` | NOT NULL, DEFAULT 'https://inspire.ec.europa.eu/codelist/WRBReferenceSoilGroupValue' | Indicates the WRB classification version. |
| `qualifierplace` | `TEXT` | NOT NULL | Attribute to indicate the placement of the Qualifier with regard to the WRB reference soil group (RSG). The placement can be in front of the RSG i.e. prefix or it can be behind the RSG i.e. suffix. |
| `wrbqualifier` | `TEXT` | NOT NULL | Name element of WRB, 2nd level of classification. |
| `wrbspecifier_1` | `TEXT` |  | First code that indicates the degree of expression of a qualifier or the depth range of which the qualifier applies. |
| `wrbspecifier_2` | `TEXT` |  | Second code that indicates the degree of expression of a qualifier or the depth range of which the qualifier applies. |


### Table Identifiers
In this table, the primary key is the *id* field (integer, auto-incrementing).  
There is also a text field named **GUID**, which stores a *UUID* (Universally Unique Identifier) compliant with RFC 4122.

Although GUID is not mandatory at the schema level (it is not declared NOT NULL), its functional requirement is enforced by two triggers:
- **wrbqualifiergrouptypeguid (INSERT)** trigger: if GUID is missing or empty, a correctly formatted UUID is generated and inserted automatically.
- **wrbqualifiergrouptypeguidupdate (UPDATE)** trigger: prevents any modification of GUID after insertion, making it immutable (effectively behaving as a stable key).  

Any foreign keys (FK) from other tables reference this table’s GUID field rather than the id field, ensuring stable and interoperable references across datasets and database instances.


### Relationships (as child)
- None

### Referenced by (as parent)
- `wrbqualifiergroup_profile.guid_wrbqualifiergrouptype` → `wrbqualifiergrouptype.guid` (**ON UPDATE** CASCADE, **ON DELETE** CASCADE)

### Indexes

| Name | Unique | Columns | Origin | Partial |
|------|--------|---------|--------|---------|
| `sqlite_autoindex_wrbqualifiergrouptype_2` | Yes | `wrbversion`, `qualifierplace`, `wrbqualifier`, `wrbspecifier_1`, `wrbspecifier_2` | `u` | No |
| `sqlite_autoindex_wrbqualifiergrouptype_1` | Yes | `guid` | `u` | No |

### Triggers
For every trigger you will find:

- **When it runs** (BEFORE/AFTER, INSERT/UPDATE/DELETE)
- **What it reads and compares** (columns, lookups in other tables)
- **What happens on success** (the statement proceeds, optional updates)
- **What happens on failure** (the exact error text raised)

 


#### `wrbqualifiergrouptypeguid` / `wrbqualifiergrouptypeguidupdate`
**When they run:** AFTER INSERT / AFTER UPDATE OF `guid`

**What they do:** Assign GUID when missing; prevent changing it later.

**If the check passes:** Insert writes GUID; unchanged updates proceed.

**If the check fails:** On change, abort with: `Cannot update guid column.`

#### `i_wrbqualifier` / `u_wrbqualifier`
**When they run:** BEFORE INSERT / BEFORE UPDATE

**What they do:** Based on `wrbversion`, pick the appropriate **year’s codelist** (current/2014/2022) and verify `wrbqualifier` exists there.

**If the check passes:** Statement proceeds.

**If the check fails:** Aborts with (verbatim): `Table wrbqualifiergrouptype: Invalid value for wrbversion. Must be present in id of the correct year codelist collection.`

#### `i_qualifierplace` / `u_qualifierplace`
**When they run:** BEFORE INSERT / BEFORE UPDATE

**What they do:** Ensure `qualifierplace` exists in **WRBQualifierPlaceValue**.

**If the check passes:** Statement proceeds.

**If the check fails:** Aborts with the invalid-value message.

#### `i_wrbspecifier_1/2` / `u_wrbspecifier_1/2`
**When they run:** BEFORE INSERT / BEFORE UPDATE

**What they do:** When provided, `wrbspecifier_1` and `wrbspecifier_2` must exist in the **WRBSpecifierValue** collection corresponding to `wrbversion`.

**If the check passes:** Statement proceeds.

**If the check fails:** Aborts with the invalid-value message.

#### `i_wrbqualversion` / `u_wrbqualversion`
**When they run:** BEFORE INSERT / BEFORE UPDATE

**What they do:** Ensure `wrbversion` exists in `codelist.id` for `collection = 'wrbversion'`.

**If the check passes:** Statement proceeds.

**If the check fails:** Aborts with (verbatim): `Table soilprofile: Invalid value for wrbversion. Must be present in id of wrbreferencesoilgroupvalue codelist.`

#### `i_unique_wrbqualifiergrouptype` / `u_unique_wrbqualifiergrouptype`
**When they run:** BEFORE INSERT / BEFORE UPDATE

**What they do:** Enforce **composite uniqueness** for the tuple `(wrbversion, qualifierplace, wrbqualifier, wrbspecifier_1, wrbspecifier_2)`.

**If the check passes:** Statement proceeds.

**If the check fails:** Aborts with: `Duplicate entry found for wrbversion, qualifierplace, wrbqualifier, wrbspecifier_1, wrbspecifier_2.`

#### `i_check_specifiers_not_equal` / `u_check_specifiers_not_equal`
**When they run:** BEFORE INSERT / BEFORE UPDATE

**What they do:** Apply two rules: (1) `wrbspecifier_1` must **not equal** `wrbspecifier_2`; (2) if `wrbspecifier_2` is not NULL, then `wrbspecifier_1` must **not be NULL**.

**If the check passes:** Statement proceeds.

**If the check fails:** Aborts with either `wrbspecifier_1 and wrbspecifier_2 must not be equal` or `wrbspecifier_1 must not be NULL when wrbspecifier_2 is not NULL`.


---

Relationships (child → parent):
-------------------------------
- [datastream].guid_soilderivedobject  -->  [soilderivedobject].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [datastream].guid_profileelement  -->  [profileelement].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [datastream].guid_soilprofile  -->  [soilprofile].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [datastream].guid_soilsite  -->  [soilsite].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [datastream].guid_observingprocedure  -->  [observingprocedure].guid  (ON UPDATE CASCADE, ON DELETE SET NULL)
- [datastream].guid_observedproperty  -->  [observedproperty].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [datastream].guid_sensor  -->  [sensor].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [datastream].guid_thing  -->  [thing].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [datastream].code_unitofmeasure  -->  [unitofmeasure].code  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [derivedprofilepresenceinsoilbody].guid_soilprofile  -->  [soilprofile].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [derivedprofilepresenceinsoilbody].guid_soilbody  -->  [soilbody].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [faohorizonnotationtype].guid_profileelement  -->  [profileelement].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [isbasedonobservedsoilprofile].guid_soilprofile  -->  [soilprofile].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [isbasedonobservedsoilprofile].guid_soilderivedobject  -->  [soilderivedobject].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [isbasedonsoilbody].guid_soilbody  -->  [soilbody].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [isbasedonsoilbody].guid_soilderivedobject  -->  [soilderivedobject].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [isbasedonsoilderivedobject].guid_related  -->  [soilderivedobject].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [isbasedonsoilderivedobject].guid_base  -->  [soilderivedobject].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [isderivedfrom].guid_related  -->  [soilprofile].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [isderivedfrom].guid_base  -->  [soilprofile].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [observation].guid_datastream  -->  [datastream].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [obsprocedure_obsdproperty].guid_observedproperty  -->  [observedproperty].guid  (ON UPDATE CASCADE, ON DELETE RESTRICT)
- [obsprocedure_obsdproperty].guid_observingprocedure  -->  [observingprocedure].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [obsprocedure_sensor].guid_sensor  -->  [sensor].guid  (ON UPDATE CASCADE, ON DELETE RESTRICT)
- [obsprocedure_sensor].guid_observingprocedure  -->  [observingprocedure].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [otherhorizon_profileelement].guid_otherhorizonnotationtype  -->  [otherhorizonnotationtype].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [otherhorizon_profileelement].guid_profileelement  -->  [profileelement].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [othersoilnametype].othersoilname  -->  [soilprofile].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [profileelement].ispartof  -->  [soilprofile].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilbody_geom].guid_soilbody  -->  [soilbody].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilplot].locatedon  -->  [soilsite].guid  (ON UPDATE CASCADE, ON DELETE NO ACTION)
- [soilprofile].location  -->  [soilplot].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [wrbqualifiergroup_profile].guid_wrbqualifiergrouptype  -->  [wrbqualifiergrouptype].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [wrbqualifiergroup_profile].guid_soilprofile  -->  [soilprofile].guid  (ON UPDATE CASCADE, ON DELETE CASCADE)

Reverse relationships (parent → children):
------------------------------------------
- [datastream].guid  -->  [observation].guid_datastream  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [observedproperty].guid  -->  [datastream].guid_observedproperty  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [observedproperty].guid  -->  [obsprocedure_obsdproperty].guid_observedproperty  (ON UPDATE CASCADE, ON DELETE RESTRICT)
- [observingprocedure].guid  -->  [datastream].guid_observingprocedure  (ON UPDATE CASCADE, ON DELETE SET NULL)
- [observingprocedure].guid  -->  [obsprocedure_obsdproperty].guid_observingprocedure  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [observingprocedure].guid  -->  [obsprocedure_sensor].guid_observingprocedure  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [otherhorizonnotationtype].guid  -->  [otherhorizon_profileelement].guid_otherhorizonnotationtype  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [profileelement].guid  -->  [datastream].guid_profileelement  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [profileelement].guid  -->  [faohorizonnotationtype].guid_profileelement  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [profileelement].guid  -->  [otherhorizon_profileelement].guid_profileelement  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [sensor].guid  -->  [datastream].guid_sensor  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [sensor].guid  -->  [obsprocedure_sensor].guid_sensor  (ON UPDATE CASCADE, ON DELETE RESTRICT)
- [soilbody].guid  -->  [derivedprofilepresenceinsoilbody].guid_soilbody  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilbody].guid  -->  [isbasedonsoilbody].guid_soilbody  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilbody].guid  -->  [soilbody_geom].guid_soilbody  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilderivedobject].guid  -->  [datastream].guid_soilderivedobject  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilderivedobject].guid  -->  [isbasedonobservedsoilprofile].guid_soilderivedobject  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilderivedobject].guid  -->  [isbasedonsoilbody].guid_soilderivedobject  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilderivedobject].guid  -->  [isbasedonsoilderivedobject].guid_related  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilderivedobject].guid  -->  [isbasedonsoilderivedobject].guid_base  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilplot].guid  -->  [soilprofile].location  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilprofile].guid  -->  [datastream].guid_soilprofile  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilprofile].guid  -->  [derivedprofilepresenceinsoilbody].guid_soilprofile  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilprofile].guid  -->  [isbasedonobservedsoilprofile].guid_soilprofile  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilprofile].guid  -->  [isderivedfrom].guid_related  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilprofile].guid  -->  [isderivedfrom].guid_base  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilprofile].guid  -->  [othersoilnametype].othersoilname  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilprofile].guid  -->  [profileelement].ispartof  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilprofile].guid  -->  [wrbqualifiergroup_profile].guid_soilprofile  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilsite].guid  -->  [datastream].guid_soilsite  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [soilsite].guid  -->  [soilplot].locatedon  (ON UPDATE CASCADE, ON DELETE NO ACTION)
- [thing].guid  -->  [datastream].guid_thing  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [unitofmeasure].code  -->  [datastream].code_unitofmeasure  (ON UPDATE CASCADE, ON DELETE CASCADE)
- [wrbqualifiergrouptype].guid  -->  [wrbqualifiergroup_profile].guid_wrbqualifiergrouptype  (ON UPDATE CASCADE, ON DELETE CASCADE)

Legend:
  * column name prefixed with '*' denotes PRIMARY KEY in table boxes.
  * arrow 'child --> parent' indicates a many-to-one relationship.
  * arrow 'parent --> child' indicates the reverse view of the same FK.


## Cascade Summary
- Deleting from `datastream` may delete rows in:
  - `observation`
- Deleting from `observedproperty` may delete rows in:
  - `datastream`
- Deleting from `observingprocedure` may delete rows in:
  - `obsprocedure_obsdproperty`
  - `obsprocedure_sensor`
- Deleting from `otherhorizonnotationtype` may delete rows in:
  - `otherhorizon_profileelement`
- Deleting from `profileelement` may delete rows in:
  - `datastream`
  - `faohorizonnotationtype`
  - `otherhorizon_profileelement`
- Deleting from `sensor` may delete rows in:
  - `datastream`
- Deleting from `soilbody` may delete rows in:
  - `derivedprofilepresenceinsoilbody`
  - `isbasedonsoilbody`
  - `soilbody_geom`
- Deleting from `soilderivedobject` may delete rows in:
  - `datastream`
  - `isbasedonobservedsoilprofile`
  - `isbasedonsoilbody`
  - `isbasedonsoilderivedobject`
- Deleting from `soilplot` may delete rows in:
  - `soilprofile`
- Deleting from `soilprofile` may delete rows in:
  - `datastream`
  - `derivedprofilepresenceinsoilbody`
  - `isbasedonobservedsoilprofile`
  - `isderivedfrom`
  - `othersoilnametype`
  - `profileelement`
  - `wrbqualifiergroup_profile`
- Deleting from `soilsite` may delete rows in:
  - `datastream`
- Deleting from `thing` may delete rows in:
  - `datastream`
- Deleting from `unitofmeasure` may delete rows in:
  - `datastream`
- Deleting from `wrbqualifiergrouptype` may delete rows in:
  - `wrbqualifiergroup_profile`
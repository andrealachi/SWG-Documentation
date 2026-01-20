# Codelist

<p>&nbsp;</p>

<p>
  <img src="assets/codelist.svg"
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

## Table: `codelist`

### Columns

| Name | Type | Constraints | Description |
|------|------|-------------|-------------|
| `id` | `TEXT` |  | Codelist Voice Uri, and Primary Key of the Table. |
| `label` | `TEXT` |  | A word or phrase used to name or describe something, often to identify or classify it. |
| `collection` | `TEXT` |  | Structured set of related elements, which share common characteristics and are managed with unique and persistent identifiers. |

### Relationships (as child)
- None

### Referenced by (as parent)
- None

### Indexes
- None

### Triggers
- None

Codelists in the SO (Soil) INSPIRE domain are essential for ensuring a standardized representation of soil data across the European Union. They enable consistent classification and encoding of specific values (e.g., soil types, usage categories) across different languages and applications, ensuring interoperability and semantic integrity in environmental datasets.

> [!IMPORTANT]
> Although the codelist table has no relationship with other tables, its presence is crucial for the correct data management and control.
> ***Essentially, if a coded value is in the table, it is supposed to be valid; if not, the code is to be considered as incorrect, and the relative value isn’t stored***.

> [!NOTE]
> It includes replicates of all  SO domain valid codes extracted from the INSPIRE registry (https://inspire.ec.europa.eu/registry).

 
The presence of the codelist table in the Geopackage allows forms for displaying dropdown lists, simplifying the data entry. However, up to now (12/2025), not all the mandatory items foreseen in the INSPIRE SOIL UML structure have been populated into the INSPIRE registry. For those mandatory items foreseen in the INSPIRE SOIL UML structure for which there is not yet a published codelis inside the INSPIRE registry, that is, for WRBdiagnostichorizon, ProcessParameterNameValue, WRBReferenceSoilGroupValue (2014), WRBReferenceSoilGroupValue (2022), WRBQualifierValue (2022), and WRBSpecifierValue (2022)), we made reference to other officially mainatined controlled vocabularies by means of URI.
 
Moreover, internal codelists have also been added to the overmentioned table to manage forms more efficiently.
INSPIRE registry


## List of Tables with their respective codelists


### Table: soilsite

**Field:** `soilinvestigationpurpose`  
**Codelist:** `SoilInvestigationPurposeValue`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** http://inspire.ec.europa.eu/codelist/SoilInvestigationPurposeValue


### Table: soilplot

**Field:** `soilplottype`  
**Codelist:** `SoilPlotTypeValue`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** http://inspire.ec.europa.eu/codelist/SoilPlotTypeValue


### Table:  soilprofile

The following table lists the available values for the **`wrbversion`** **internal codelist**, which is used to identify the reference year of the WRB soil classification adopted.

| ID (URI) | Label | Collection |
|----------|-------|----------|
| https://inspire.ec.europa.eu/codelist/WRBReferenceSoilGroupValue | WRB 2006 | wrbversion |
| https://agroportal.lirmm.fr/ontologies/WRB_2014-2015 | WRB 2014 | wrbversion |
| https://obrl-soil.github.io/wrbsoil2022/ | WRB 2022 | wrbversion |

Codelists used for the `wrbreferencesoilgroup` field depending on the selected wrbversion.

- WRBReferenceSoilGroupValue (2006)
CODELIST ***INSPIRE***
http://inspire.ec.europa.eu/codelist/WRBReferenceSoilGroupValue

- WRBReferenceSoilGroupValue (2014)
CODELIST AGROPORTAL
https://agroportal.lirmm.fr/ontologies/AGROVOC/

- WRBReferenceSoilGroupValue (2022)
CODELIST ORBL-SOIL
https://obrl-soil.github.io/wrbsoil2022/


### Table: othersoilnametype

**Field:** `othersoilname_type`  
**Codelist:** `OtherSoilNameTypeValue`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** https://inspire.ec.europa.eu/codelist/OtherSoilNameTypeValue


### Table: profileelement

**Field:** `layertype`  
**Codelist:** `LayerTypeValue`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** http://inspire.ec.europa.eu/codelist/LayerTypeValue

**Field:** `<field_name>`  
**Codelist:** `layerrocktype`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** http://inspire.ec.europa.eu/codelist/LithologyValue

**Field:** `<field_name>`  
**Codelist:** `layergenesisprocess`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** http://inspire.ec.europa.eu/codelist/EventProcessValue

**Field:** `layergenesisenviroment`  
**Codelist:** `EventEnvironmentValue`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** http://inspire.ec.europa.eu/codelist/EventEnvironmentValue

**Field:** `layergenesisprocessstate`  
**Codelist:** `LayerGenesisProcessStateValue`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** http://inspire.ec.europa.eu/codelist/LayerGenesisProcessStateValue


### Table: faohorizonnotationtype

**Fields:** `faohorizonmaster_1` -`faohorizonmaster_2`  
**Codelist:** `FAOHorizonMaster`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** https://inspire.ec.europa.eu/codelist/FAOHorizonMasterValue

**Fields:** `faohorizonsubordinate_1` - `faohorizonsubordinate_2`  - `faohorizonsubordinate_3`   
**Codelist:** `FAOHorizonSubordinate`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** https://inspire.ec.europa.eu/codelist/FAOHorizonSubordinateValue

**Field:** `faoprime`  
**Codelist:** `FAOPrime`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** http://inspire.ec.europa.eu/codelist/FAOPrimeValue


### Table: otherhorizonnotationtype


## INTERNAL to do

**Field:** `horizonnotation`  
**Codelist:** `OtherHorizonNotationTypeValue`  
**Codelist Authority:** ORBL‑SOIL  
**Codelist URL:** https://obrl-soil.github.io/wrbsoil2022/chapter-03.html#sec-diagh


**Field:** `diagnostichorizon`  
**Codelist:** `WRBdiagnostichorizon`  
**Codelist Authority:** ORBL‑SOIL  
**Codelist URL:** https://obrl-soil.github.io/wrbsoil2022/chapter-03.html#sec-diagh


### Table: wrbqualifiergrouptype

**Field:** `qualifierplace`  
**Codelist:** `WRBQualifierPlaceValue`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** http://inspire.ec.europa.eu/codelist/WRBQualifierPlaceValue

**Field:** `wrbqualifier`  
**Codelist:** `WRBQualifierValue (2006)`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** http://inspire.ec.europa.eu/codelist/WRBQualifierValue

**Field:** `wrbqualifier`  
**Codelist:** `WRBQualifierValue (2022)`  
**Codelist Authority:** ORBL‑SOIL  
**Codelist URL:** https://obrl-soil.github.io/wrbsoil2022/

**Field:** `wrbqualifier`  
**Codelist:** `WRBSpecifierValue (2006)`  
**Codelist Authority:** INSPIRE  
**Codelist URL:** http://inspire.ec.europa.eu/codelist/WRBSpecifierValue

**Fields:** `wrbspecifier_1` - `wrbspecifier_2` 
**Codelist:** `WRBSpecifierValue (2022)`  
**Codelist Authority:** ORBL‑SOIL  
**Codelist URL:** https://obrl-soil.github.io/wrbsoil2022/

##  How to Populate the `codelist` Table for Codespace Management

This document provides technical guidelines for correctly populating the `codelist` table in a **GeoPackage/SQLite database**.  
Proper population of this table ensures that **triggers** defined in the GeoPackage work as intended and that codespaces are managed consistently.

### Why Is This Important?

The `codelist` table contains all **INSPIRE code lists** required to manage many tables in the GeoPackage.  
In this context, it is also used to store **codespaces** and their corresponding values.

The table will include:
- **Codespace definitions** (e.g., `coatingNatureValueCode`)
- **Codespace elements** (e.g., `…-C`, `…-CC`, etc.)

>**Rule:** Always insert the **codespace list first** (top-level), then its **elements**.


## Workflow Overview

### Step 1 — Insert the Codespace List (Top-Level)

Each codespace list requires:
- `id`: **A unique URI or key for the list**
- `label`: A human-readable name for the list
- `collection`: Always set to **`'Category'`**

**Example:**
```sql
INSERT INTO codelist (id, label, collection)
VALUES ('http://w3id.org/glosis/model/codelists/coatingNatureValueCode',
        'coatingNatureValueCode',
        'Category');
```

### Step 2 — Insert the Codespace Elements

For each element in the codespace:
- `id`: A unique URI or key for the element (e.g., …-C, …-CC)
- `label`: A descriptive label for the element
- `collection`: **The id of the codespace list created in Step 1**

**Example:**
```sql
INSERT INTO codelist (id, label, collection) 
VALUES('http://w3id.org/glosis/model/codelists/coatingNatureValueCode-C',  
	   'Clay',                
	   'http://w3id.org/glosis/model/codelists/coatingNatureValueCode'),
INSERT INTO "codelist" (id, label, collection) 
VALUES ('http://w3id.org/glosis/model/codelists/coatingNatureValueCode-CC', 
		'Calcium carbonate', 
		'http://w3id.org/glosis/model/codelists/coatingNatureValueCode'),
INSERT INTO "codelist" (id, label, collection) 
VALUES ('http://w3id.org/glosis/model/codelists/coatingNatureValueCode-CH', 
		'Clay and humus (organic matter)', 
		'http://w3id.org/glosis/model/codelists/coatingNatureValueCode');
```
### Resulting Table Structure
After executing the above statements, the codelist table will look like this:

| id | label | collection |
|------|------|-----------|
| http://w3id.org/glosis/model/codelists/coatingNatureValueCode | coatingNatureValueCode| Category |
| http://w3id.org/glosis/model/codelists/coatingNatureValueCode-C | Clay| http://w3id.org/glosis/model/codelists/coatingNatureValueCode |
| http://w3id.org/glosis/model/codelists/coatingNatureValueCode-CC | Calcium carbonate| http://w3id.org/glosis/model/codelists/coatingNatureValueCode |
| http://w3id.org/glosis/model/codelists/coatingNatureValueCode-CH | Clay and humus (organic matter)| http://w3id.org/glosis/model/codelists/coatingNatureValueCode |

---

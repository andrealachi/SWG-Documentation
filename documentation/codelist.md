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
 

## List of Features with their respective codelists:



### FEATURE soilsite

SoilInvestigationPurposeValue
CODELIST ***INSPIRE***
http://inspire.ec.europa.eu/codelist/SoilInvestigationPurposeValue


### FEATURE soilplot

SoilPlotTypeValue
CODELIST ***INSPIRE***
http://inspire.ec.europa.eu/codelist/SoilPlotTypeValue


### FEATURE  soilprofile

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


### FEATURE othersoilnametype

OtherSoilNameTypeValue
CODELIST ***INSPIRE***
https://inspire.ec.europa.eu/codelist/OtherSoilNameTypeValue 


### FEATURE profileelement

LayerTypeValue
CODELIST ***INSPIRE***
http://inspire.ec.europa.eu/codelist/LayerTypeValue

LithologyValue
CODELIST ***INSPIRE***
http://inspire.ec.europa.eu/codelist/LithologyValue

EventProcessValue
CODELIST ***INSPIRE***
http://inspire.ec.europa.eu/codelist/EventProcessValue

EventEnvironmentValue
CODELIST ***INSPIRE***
http://inspire.ec.europa.eu/codelist/EventEnvironmentValue

LayerGenesisProcessStateValue
CODELIST ***INSPIRE***
http://inspire.ec.europa.eu/codelist/LayerGenesisProcessStateValue


### FEATURE faohorizonnotationtype

FAOHorizonMaster
CODELIST ***INSPIRE***
https://inspire.ec.europa.eu/codelist/FAOHorizonMasterValue

FAOHorizonSubordinate
CODELIST ***INSPIRE***
https://inspire.ec.europa.eu/codelist/FAOHorizonSubordinateValue

FAOPrime
CODELIST ***INSPIRE***
http://inspire.ec.europa.eu/codelist/FAOPrimeValue


### FEATURE otherhorizonnotationtype

WRBdiagnostichorizon
CODELIST ORBL
https://obrl-soil.github.io/wrbsoil2022/chapter-03.html#sec-diagh


### FEATURE wrbqualifiergrouptype

WRBQualifierPlaceValue
CODELIST ***INSPIRE***
http://inspire.ec.europa.eu/codelist/WRBQualifierPlaceValue

WRBQualifierValue (2006)
CODELIST ***INSPIRE***
http://inspire.ec.europa.eu/codelist/WRBQualifierValue

WRBQualifierValue (2022)
CODELIST ORBL-SOIL
https://obrl-soil.github.io/wrbsoil2022/

WRBSpecifiers (2006)
CODELIST ***INSPIRE*** 
http://inspire.ec.europa.eu/codelist/WRBSpecifierValue (Under review)

WRBSpecifierValue (2022)
CODELIST ORBL-SOIL
https://obrl-soil.github.io/wrbsoil2022/



---
<a id="datastream"></a>

# INSPIRE Soil – Required Fields

This document lists the **REQUIRED fields** for each table, following the exact formatting requested.

## soilsite
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `geometry`: POLYGON 
- `Soil Investigation Purpose`: TEXT (codelist)
- `Valid From`: DATETIME (default: today)
- `Begin Lifespan version`: DATETIME (default: today)

## soilplot
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `soilplotlocation`: POINT
- `Soil Plot Type`: TEXT (codelist)
- `Begin Lifespan version`: DATETIME (default: today)

## soilprofile
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `Begin Lifespan version`: DATETIME (default: today)
- `Valid From`: DATETIME (default: today)
- `isderived`: BOOLEAN (default: 0)
- `isoriginalclassification`: BOOLEAN (default: 1)

## othersoilnametype
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `othersoilname_type`: TEXT (codelist)
- `isoriginalclassification`: BOOLEAN (default: 0)

## isderivedfrom
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `guid_base`: TEXT (Derived Profile)
- `guid_related`: TEXT (Observed Profile)

## soilbody
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `Begin Lifespan version`: DATETIME (default: today)
- `soilbodylabel`: TEXT

## soilbody_geom
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `geom`: MULTIPOLYGON
- `guid_soilbody`: TEXT (FK)

## derivedprofilepresenceinsoilbody
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `guid_soilbody`: TEXT (FK)
- `guid_soilprofile`: TEXT (FK)

## soilderivedobject
### REQUIRED fields
- `id`: primary key (auto-incrementing)

## isbasedonobservedsoilprofile
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `guid_soilderivedobject`: TEXT (FK)
- `guid_soilprofile`: TEXT (FK)

## isbasedonsoilbody
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `guid_soilderivedobject`: TEXT (FK)
- `guid_soilbody`: TEXT (FK)

## isbasedonsoilderivedobject
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `guid_base`: TEXT (FK)
- `guid_related`: TEXT (FK)

## profileelement
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `Begin Lifespan version`: DATETIME (default: today)
- `profileelementtype`: BOOLEAN (default: 0)
- `ispartof`: TEXT (FK to soilprofile.guid)
- `Depth range`: at least one of upper/lower value must be NOT NULL (rule)

## faohorizonnotationtype
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `faohorizonmaster_1`: TEXT (codelist)
- `faoprime`: TEXT (codelist)
- `isoriginalclassification`: BOOLEAN (default: 0)

## otherhorizonnotationtype
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `horizonnotation`: TEXT (codelist)
- `isoriginalclassification`: BOOLEAN (default: 0)

## otherhorizon_profileelement
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `guid_profileelement`: TEXT (FK)
- `guid_otherhorizonnotationtype`: TEXT (FK)

## wrbqualifiergrouptype
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `wrbversion`: TEXT (default: INSPIRE WRB RSG URI)
- `qualifierplace`: TEXT (codelist)
- `wrbqualifier`: TEXT (codelist)

## wrbqualifiergroup_profile
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `guid_soilprofile`: TEXT (FK)
- `guid_wrbqualifiergrouptype`: TEXT (FK)
- `qualifierposition`: INTEGER

## datastream
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `name`: TEXT
- `type`: TEXT (constrained values)
- `guid_sensor`: TEXT (FK)
- `guid_observedproperty`: TEXT (FK)

## unitofmeasure
### REQUIRED fields
- `id`: primary key (auto-incrementing)

## observedproperty
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `name`: TEXT
- `definition`: TEXT (URI)
- `source`: TEXT (default: Local)

## sensor
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `name`: TEXT

## thing
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `name`: TEXT

## observation
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `phenomenontime_start`: DATETIME (default: today)
- `guid_datastream`: TEXT (FK)

## observingprocedure
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `name`: TEXT

## obsprocedure_obsdproperty
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `guid_observingprocedure`: TEXT (FK)
- `guid_observedproperty`: TEXT (FK)

## obsprocedure_sensor
### REQUIRED fields
- `id`: primary key (auto-incrementing)
- `guid_observingprocedure`: TEXT (FK)
- `guid_sensor`: TEXT (FK)

## codelist
### REQUIRED fields
- `id`: TEXT


![Soilvise poster](assets/soilvise_poster.webp)

# Introduction to the **Soilwise** GeoPackage (SQLite)

The **Soilwise** GeoPackage is the relational (SQLite‑based) container adopted by the **SoilWise** project (Horizon Europe – Mission Soil) to enable **exchange, storage, and GIS‑native use** of soil data, with the explicit goal of making them **FAIR** and reusable across European policies, research, and land‑management workflows.[^1][^2] SoilWise provides an **integrated, actionable entry point** to scattered soil data and knowledge, recognising existing workflows and repositories and connecting to them through a **modular, scalable** architecture designed to last beyond the project horizon and co‑created with stakeholders.[^1][^2]

## Why GeoPackage/SQLite

**GeoPackage** is an **OGC** open, portable, self‑contained standard for geospatial data. Being an SQLite container, it allows **direct** use of vector features, rasters/tiles and attribute data in a **single file**, without intermediate format translations. This makes it ideal for GIS environments and for constrained connectivity scenarios.[^3][^4]  
Within the INSPIRE ecosystem, an **endorsed Good Practice** describes how to design and publish **INSPIRE datasets encoded as GeoPackage**, while preserving legal and technical compliance with the Implementing Rules (with technical conformity demonstrable through transformation to the default encoding, **GML**). The Good Practice promotes logical models **optimised for GIS usability**, with explicit mapping rules from the INSPIRE conceptual model.[^5][^6]

## Continuity with INSPIRE: from EJP SOIL to **Soilwise**

The **Soilwise** database builds upon—and updates—the work carried out around INSPIRE, including the **EJP SOIL** GeoPackage template for the **Soil (SO)** theme. That template focused on **semantic harmonisation**, **code‑list management**, and **repeatable transformations**, and is a relevant baseline for Soilwise’s relational modelling approach.[^7] This direction aligns with community guidance on publishing INSPIRE data as a **relational database** (GeoPackage as a specialisation of SQLite), including recipes and patterns for harmonisation and publication.[^8]

## Data architecture and governance

In line with the **INSPIRE Good Practice**, each GeoPackage logical schema should be accompanied by:
- a **model description** (including benefits and limitations compared to the default encoding);
- an **empty GeoPackage template** for distribution;
- an **executable specification** for model transformation (**UML→GPKG** or **GML→GPKG**), aligned with the generic INSPIRE transformation rules.[^5]

This approach ensures **traceable compliance**, supports reuse across thematic communities, and allows **use‑case‑specific** logical models while preserving a coherent methodological and semantic framework.[^5]

## Integration with **OGC SensorThings API 2.0 (STA2)**

Within SoilWise, an implementation of **OGC SensorThings API 2.0 (STA2)** is used to expose **observations** and **metadata** as **interoperable time series** via HTTP and MQTT, consistent with **O&M / ISO 19156:2023**. The **STA2** draft (OGC document **23‑019**) updates the data model and bindings (e.g., OData 4.01 alignment), strengthening the API’s role as a standards‑based **“data‑in‑motion”** complement to the GeoPackage’s **“data‑at‑rest”** layer.[^9][^10]  
The growing adoption of SensorThings for observational data (well beyond narrow IoT use) is documented by OGC/WMO community materials, which highlight alignment with the renewed **OMS/ISO 19156:2023** standard and integration with GIS tools.[^11][^12]

## QGIS usage and custom forms

The **Soilwise** GeoPackage is **natively supported by QGIS**, enabling editing, styling, and map production with no format conversion. To improve data‑entry quality and speed, we recommend configuring **custom attribute forms** (widgets, value map/relation, constraints and expressions) and adopting a **design of tabs and groups** aligned with project code lists and validation processes.[^13][^14]



[^1]: **SoilWise – project website** (Horizon Europe – Mission Soil).  
https://soilwise-he.eu/

[^2]: **Mission Soil Platform (EC) – SoilWise project page**.  
https://mission-soil-platform.ec.europa.eu/project-hub/soilwise

[^3]: **GeoPackage.org** – Overview and resources.  
https://www.geopackage.org/

[^4]: **OGC – GeoPackage Standard** (publications page).  
https://www.ogc.org/standards/geopackage/

[^5]: **INSPIRE Knowledge Base** – *GeoPackage encoding of INSPIRE datasets* (Good Practice).  
https://knowledge-base.inspire.ec.europa.eu/geopackage-encoding-inspire-datasets_en

[^6]: **INSPIRE‑MIF – gp‑geopackage‑encodings** (Good Practice repository).  
https://github.com/INSPIRE-MIF/gp-geopackage-encodings

[^7]: **EJP SOIL – INSPIRE Soil GeoPackage template** (GitHub).  
https://github.com/ejpsoil/inspire_soil_gpkg_template

[^8]: **Soil data guidance** – *INSPIRE Soil in a relational database* (recipe & workflow).  
https://ejpsoil.github.io/soildata-assimilation-guidance/cookbook/glosis-db.html

[^9]: **OGC – SensorThings API 2.0 (draft 23‑019)**.  
https://hylkevds.github.io/23-019/23-019.html

[^10]: **OGC Publications – SensorThings API** (Part 1/1.1 Sensing, Part 2 Tasking Core, STAplus).  
https://www.ogc.org/standards/sensorthings/

[^11]: **WMO/OGC** – *Integrating new OGC Standards* (slides).  
https://wmo.int/sites/default/files/2024-02/Integration%20of%20new%20OGC%20Standards%20e.g%20SensorThings%20API.pdf

[^12]: **BRGM HAL** – *SensorThings API and WaterML2.0* (slides).  
https://brgm.hal.science/hal-04456540v2/file/SensorThings%20API%20and%20%20WaterML2.0%20.pdf

[^13]: **QGIS Training Manual** – *Forms / Attribute Forms*.  
https://docs.qgis.org/latest/en/docs/training_manual/create_vector_data/forms.html

[^14]: **QField Documentation** – *Simple attribute form configuration*.  
https://docs.qfield.org/how-to/project-setup/attributes-form/
``



# Database Structure Report


## Index
- [codelist](codelist.md)
- [datastream](datastream.md)
- [derivedprofilepresenceinsoilbody](derivedprofilepresenceinsoilbody.md)
- [faohorizonnotationtype](faohorizonnotationtype.md)
- [isbasedonobservedsoilprofile](isbasedonobservedsoilprofile.md)
- [isbasedonsoilbody](isbasedonsoilbody.md)
- [isbasedonsoilderivedobject](isbasedonsoilderivedobject.md)
- [isderivedfrom](isderivedfrom.md)
- [observation](observation.md)
- [observedproperty](observedproperty.md)
- [observingprocedure](observingprocedure.md)
- [obsprocedure_obsdproperty](obsprocedure_obsdproperty.md)
- [obsprocedure_sensor](obsprocedure_sensor.md)
- [otherhorizon_profileelement](otherhorizon_profileelement.md)
- [otherhorizonnotationtype](otherhorizonnotationtype.md)
- [othersoilnametype](othersoilnametype.md)
- [profileelement](profileelement.md)
- [sensor](sensor.md)
- [soilbody](soilbody.md)
- [soilbody_geom](soilbody_geom.md)
- [soilderivedobject](soilderivedobject.md)
- [soilplot](soilplot.md)
- [soilprofile](soilprofile.md)
- [soilsite](soilsite.md)
- [thing](thing.md)
- [unitofmeasure](unitofmeasure.md)
- [wrbqualifiergroup_profile](wrbqualifiergroup_profile.md)
- [wrbqualifiergrouptype](wrbqualifiergrouptype.md)




# Open Geopackage Soilwise in QGis

## Introduction

The **SoilWise GeoPackage** contains an **embedded QGIS project** named **`PRJ_SO`** that exposes **preconfigured custom forms** to guide users in **data entry** and **visualization** [^1].

This capability is available **even in the “empty” GeoPackage**, providing a consistent, ready‑to‑use working environment from the very beginning.

From a technical standpoint, the QGIS project is **stored directly inside the GeoPackage** in the **`qgis_projects`** table; each project is saved as **textual content** representing a **QGZ archive** (a zipped package) which, in turn, contains the **`.qgs` project file in XML format** [^2][^3]. This ensures that the project and the data live in the **same `.gpkg` file**, preserving styles, layouts, views, and settings as defined by SoilWise.

Opening is **one‑click**: select the **embedded project** from the GeoPackage and the SoilWise environment loads exactly as designed, with **forms, styles, and settings** ready to use.

## Opne PRJ_SO project

Open Qgis.

In the Browser Tab, right-click on the
Geopackage icon ① and click on “New
Connection”. ②

Search the file system for the Geopackage
file you want to use.

Inside the newly connected file, ③ you will
find the **`PRJ_SO`**project. ④

Double-click on it, and the project will open.



---

[^1]: **SoilWise – project website** (Horizon Europe – Mission Soil).  
https://soilwise-he.eu/

[^2]: **QGIS User Manual – Working with Project Files**.  
https://docs.qgis.org/latest/en/docs/user_manual/introduction/project_files.html

[^3]: **GIS StackExchange – “Save as Project vs. Save as GeoPackage?”** (comment on how QGIS stores the project in the `qgis_projects` table as a zipped QGZ/XML).  
https://gis.stackexchange.com/questions/457189/save-as-project-vs-save-as-geopackage

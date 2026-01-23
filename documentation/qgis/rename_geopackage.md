
# How to Rename the Soilwise GeoPackage


When a QGIS project is saved inside a GeoPackage, it stores **hard‑coded paths and references to the file using its exact filename**. These references are not dynamic—QGIS does not automatically update them if the file name changes.

As a result, **renaming the GeoPackage alters a critical part of the stored path (the filename)**, preventing QGIS from locating the layers contained within it. This causes **all internal links to break**, leading to data source errors and, in many cases, preventing the project from loading correctly.

>[!WARNING]
>Even a simple change to the `.gpkg` filename is enough to break the entire referencing system the project relies on, because all paths continue to point to the original name.


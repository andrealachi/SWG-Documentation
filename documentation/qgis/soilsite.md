# Soil Site Forms

To open the Soil Site custom forms, go to the Layers panel, right‑click the `soilsite` layer, and choose **Open Attribute Table** from the context menu.

> [!TIP]
> For further information on the custom forms, consult the documents [Customized Attribute Forms in QGIS](./custom_form.md) and [Navigating GeoPackage Tables via Forms](./navigating_via_form.md)  

## ID GROUP

### Fields

- **`id`** - Primary **AUTO_INCREMENT INTEGER PRIMARY KEY**; it’s the required identifier for GeoPackage tables and is assigned automatically on insert.

- **`guid`** - **Global identifier** in UUID format, stored as text.This field is **optional**, but in this project it is **automatically managed via triggers**.

> [!IMPORTANT]
> On opening, the **ID** group is collapsed: there’s no need to edit these manually because **both fields are system‑managed** (`id` by the SQLite engine, `guid` by triggers), reducing errors and ensuring identifier consistency over time.

## INSPIRE ID GROUP

>An **INSPIRE ID** is the **external unique identifier** assigned to each spatial object in INSPIRE datasets; it ensures **uniqueness** and **persistence** and allows external applications to reliably reference the same object over time.
The identifier **must not be changed** during the object’s life cycle; it can also be published as a **URI** to facilitate web-based referencing. [^1]

[^1]: Creating INSPIRE external unique object identifiers in the scope of the END reporting.
https://epanet.eea.europa.eu/Eionet/reportnet/docs/noise/guidelines/inspire_identifiers_doc.pdf 

### Fields

- **`localId`** — Local identifier assigned by the data provider; **unique** within its namespace.
- **`namespace`** — Namespace that uniquely identifies the data source/domain of the spatial object. 
- **`versionId`** — Identifier of the specific **version** of the object; **optional (voidable)** and used to distinguish different versions of the same object. 

> [!IMPORTANT]
> These fields are not mandatory, but **filling them out is strongly recommended**: they help uniquely identify the record in forms and across data exchanges.  
> In particular, `localid` + `namespace` form a stable identifier; `versionid` helps track changes over time.


## REQUIRED fields

- `id` primary key, autoincrement
- `geometry` POLYGON
- `soilinvestigationpurpose` TEXT
- `validfrom` DATETIME  DEFAULT Today
- `beginlifespanversion` DATETIME  DEFAULT Today




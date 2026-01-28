# Soil Site Forms


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










> [!WARNING]
>The contents of the buffer are temporary: if the editing session ends without saving, the buffer is discarded.


[^1]: Creating INSPIRE external unique object identifiers in the scope of the END reporting.
https://epanet.eea.europa.eu/Eionet/reportnet/docs/noise/guidelines/inspire_identifiers_doc.pdf



## Committing Changes (Save Layer Edits)
Saving occurs when the user clicks **Save Layer Edits** <img src="../assets/save.webp">. At this stage, QGIS performs a **commit** of the edit buffer to the underlying data source (GeoPackage, Shapefile, PostGIS, etc.).

During the commit:

- the buffer is processed and its operations are converted into write actions,  
- QGIS performs **data validation** (constraints, required fields, data types, domains, unique values, parent–child relationships),  
- any validation errors are reported before the commit, preventing inconsistent data from being written.

If validation succeeds, the buffer is cleared and the dataset is updated.

> [!CAUTION]
>**Errors are displayed in a banner within the map canvas**. However, the banner may not always be visible when some panels are in a floating (undocked) state, which can partially cover or obscure the map canvas.
Make sure to check the map canvas if error messages are not immediately visible.

> [!TIP]
> In case of an error, to understand its cause it is necessary to locate the corresponding message directly within the **trigger section** of the table associated with the form where the error occurred.



## Rolling Back Changes
When editing mode is disabled <img src="../assets/pencil.webp">, QGIS prompts the user to either:

- **save** pending changes, or  
- **discard** them.

If the user chooses not to save, QGIS performs a **rollback**:

- the edit buffer is removed,  
- all unsaved modifications are discarded,  
- the layer returns to its original on‑disk state.

This process is similar to rolling back a transaction in a relational database.



## Handling Parent–Child Relationships
When a layer is part of a relational structure (e.g., a GeoPackage with related tables), forms may include sub‑forms for editing child records.

Technically:

- each involved layer maintains its **own edit buffer**,  
- modifications to child records are not saved automatically when the parent layer is saved,  
 

## Persistence and Disk Writing
At the end of a successful commit:

- QGIS writes all changes to the GeoPackage (SQLite) or the target database,  
- indexes and internal metadata structures are updated,  
- any database‑level triggers or constraints are executed.

The dataset now reflects the final state of the editing session.





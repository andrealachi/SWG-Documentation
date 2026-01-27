# Customized Attribute Forms in QGIS

## Overview

In QGIS, customized attribute forms allow full control over the layout and behavior of the user interface used for viewing, inserting, and editing attribute data.  
Instead of relying on automatically generated forms, a clearer and more structured layout has been defined, enabling precise control over which fields are displayed, how they are grouped, and under which conditions they are visible.

Each field has been associated with a specific **widget**, i.e., a control designed to facilitate data entry and reduce user errors.

---

## Available Widgets

The main widget types used include:

- **Text Edit** – free‑text input  
- **Checkbox** – boolean values  
- **Date/Time** – date and time selection  
- **Value Map / Value Relation** – predefined lists of values (fixed or derived from another table)  
- **Relation Reference / Relation Editor** – handling one‑to‑many or many‑to‑many table relationships  
- **HTML / Text / Spacer** – support elements for notes, layout structure, or separators  

These widgets allow the form to guide the user and enforce the data‑entry rules defined in the underlying data model.

---

## Purpose of Customized Forms

Customized forms are used to:

- guide the user through data entry with a clear and structured layout  
- apply constraints, conditional visibility, and default values  
- ensure consistency with the data model  
- display enriched information (images, documents, related records)

The goal is to provide a functional interface, more efficient and clearer than the default one, particularly suitable for complex datasets.

---

# Interface Elements Related to Forms

## Record List Panel (Left Side)

On the left side of the interface, a panel lists all the records belonging to the layer.  
Selecting a record from this list displays its attribute content on the right-hand side, using the associated customized form.

---

## “Form” and “Table” Toolbar Buttons

The attribute toolbar provides two main viewing modes:

- **Form View**  
  Displays the attribute form for a single record at a time.

- **Table View**  
  Shows the attributes in tabular mode, useful for multi-record inspection and quick selection operations.

These two modes correspond to the official QGIS Attribute Table viewing options.

---

## Selection Tools

Selection tools determine which records can be opened or edited through the form.  
The main selection options include:

- **Select Feature** / **Select by Rectangle** / **Select by Polygon** / **Select by Freehand**  
- **Select by Expression**  
- **Filter Selected** / **Show Selected Features** (within the attribute table)

Selection is an essential part of the editing workflow, as it controls which records are loaded into the form.

---

## Additional Information

For more details, refer to the official QGIS documentation:  
https://docs.qgis.org/3.44/en/docs/user_manual/working_with_vector/attribute_table.html
``




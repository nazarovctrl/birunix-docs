---
icon: input-pipe
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# b-dynamic-field

## Overview

The `<b-dynamic-field>` directive allows to render & update & customize special dynamic-field hashmap from back-end which is loaded from KDYN module.

## Usage

To use the `b-dynamic-field` directive, follow these steps:

1. Load `field` special hashmap from back-end which is loaded from KDYN module.
2.  Include the `b-dynamic-field` directive in your HTML template, and assign the loaded hashmap to `local-data` attribute (required).

    ```html
    <b-dynamic-field local-data="d.fields"></b-dynamic-field>
    ```

## Attributes

* `local-data` - (Required) A hashmap which is loaded from KDYN module.
* `editable` - (Optional) A boolean attribute indicating whether the fields are editable. Defaults to `"false"`.
* `size` - (Optional) The size of the column for each field. Defaults to `"24"`.

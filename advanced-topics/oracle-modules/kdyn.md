---
icon: link-slash
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

# KDYN

## Overview

The KDYN module is the back-end side of `<b-dynamic-field>` directive in client-app. Allows to create dynamic fields for entities which can be used in add/edit/view forms and reports.

## Setting up

To create dynamic fields, follow these steps:

1.  Create an **entity**:

    ```plsql
    z_Kdyn_Entities.Save_One(             i_Entity_Id    => Kdyn_Entities_Sq.Nextval,
     /* entity name                    */ i_Entity_Name  => 'Прием на работу',
     /* project code                   */ i_Project_Code => 'vhr',
     /* code used for reports          */ i_Code         => 'hiring',
     /* pcode used to reference entity */ i_Pcode        => 'VHR:HIRING');
    ```

    Also, you can save this into your **settings.sql** inside your module.
2.  Now, implement **loading** & **saving** these dynamic fields in your entity's forms:\
    in **Add\_Model** function:

    ```plsql
    Kdyn_Util.Entity_Fields_Load(        i_Company_Id   => Ui.Company_Id,
     /* previously saved entity pcode */ i_Entity_Pcode => 'VHR:HIRING');
    ```

    in **Save** procedure:

    ```plsql
    Kdyn_Api.Filled_Fields_Save_Procedure(    i_Company_Id    => Ui.Company_Id,
     /* Previously saved entity pcode      */ i_Entity_Pcode  => 'VHR:HIRING',
     /* Particular row of an entity        */ i_Source_Id     => v_Hiring_Id,
     /* Hashmap which is load in Add_model */ i_Filled_Fields => v_Hiring.o_Hashmap('fields'));
    ```

    in **Edit\_Model** procedure:

    ```plsql
    Kdyn_Util.Entity_Fields_Load(        i_Company_Id   => Ui.Company_Id,
     /* previously saved entity pcode */ i_Entity_Pcode => 'VHR:HIRING',
     /* Particular row of an entity   */ i_Source_Id => v_Hiring_Id);
    ```
3. Include the `b-dynamic-field` directive in your HTML template, see b-dynamic-field directive docs.

## Usage with reports

```plsql
v_Fields := Kdyn_Util.Entity_Fields_Load_Minified(i_Company_Id   => Ui.Company_Id,
                                                  i_Entity_Pcode => 'VHR:HIRING',
                                                  i_Source_Id    => v_Hiring_Id);
/* v_Fields is a matrix_varchar2 of:
  [
    [code, name, value],
    ...
  ]
*/
```

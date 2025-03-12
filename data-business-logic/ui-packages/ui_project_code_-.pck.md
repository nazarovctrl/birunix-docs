---
icon: box-open
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

# UI\_PROJECT\_CODE\*.pck

The UI\_PROJECT\_CODE\*.pck packages act as an interface layer, bridging external requests (e.g., API calls) with the core business logic in module packages. These files reside within the ui subdirectory of their respective module folder, ensuring a clear separation of presentation and logic.

```
📁 task-manager/
├── 📁 main/
    ├── 📁 oracle/
        ├── 📁 module/
        │   ├── 📁 tmd/
        │       └── <tmd module files>
        ├── 📁 ui/
            ├── 📁 tmd /
                ├── department_list.pck
                └── <other tmd module ui packages>
```

### **Structure and Naming**

* Forms displaying tables should be "\_list" at the end of the name (e.g., department`_list`, `region_list`)
* Reports with similar purposes should be numbered (e.g., `invoice_one`, `invoice_two`)

### **Data Operations**

* Direct DML and DDL operations on tables from UI are prohibited (except SELECT)
* Temporary tables in UI are named with the prefix "ui\_path\_code" (e.g., `ui_2374_quantities`)
* All reports should include ORDER BY for sorting

### **Translation and Internationalization**

* When a UI package function calls the b package's translate (`b.translate()`) function, it prefixes the package name to the first parameter. For example: `b.Translate('UI-MANAGEMENT:' || i_Message, i_P1, i_P2, i_P3, i_P4, i_P5)`

### Example

Let's create `department_list.pck`, which contains the function `Query`. The `Query` function returns rows from the `tmd_departments` table where `company_id` matches `Ui.Company_Id`. (`Ui.Company_Id` retrieves the `company_id` of the currently logged-in user.)

The `Query` function utilizes `Fazo_Query`, which is a dynamic query. Due to its dynamic nature, we need to include the `Validation` procedure in the package body. **(Note: This should be written in the package body, not in the package specification!)**

In the UI, it is essential to implement **Validation** for dynamic queries. **Validation** is a feature introduced by GWS developers to enhance reliability and simplify the development process. Its primary purpose is to verify the existence of objects referenced in the code. If the code includes non-existent objects or those with restricted access, an error will be raised during the compilation of the `Validation` procedure.

**Validation is performed in two ways:**

1. Using the `uie.x()` function. Example: `Uie.x(Ui.Filial_Id);`
2. Performing an `update` operation.

{% code title="department_list.pck" %}
```
create or replace package Ui_Management123 is
  ----------------------------------------------------------------------------------------------------
  Function Query return Fazo_Query;
end Ui_Management123;
/
create or replace package body Ui_Management123 is
  ----------------------------------------------------------------------------------------------------
  Function Query return Fazo_Query is
    q        Fazo_Query;
  begin
    q := Fazo_Query('tmd_departments',
                    Fazo.Zip_Map('company_id',
                                 Ui.Company_Id),
                    true);
    
    q.Number_Field('department_id', 'manager_id');
    q.Varchar2_Field('name','state');
    q.Date_Field('created_on');
    
    q.Refer_Field('manager_name',
                  'manager_id',
                  'md_persons',
                  'person_id',
                  'name',
                  'select k.person_id, k.name
                     from md_persons k
                    where k.company_id = :company_id');
    
    q.Option_Field('state_name',
                   'state',
                   Array_Varchar2('A', 'P'),
                   Array_Varchar2(Ui.t_Active, Ui.t_Passive));
    return q;
  end;
  
  ----------------------------------------------------------------------------------------------------
  Procedure Validation is 
  begin  
  update Tmd_Departments
     set Company_Id    = null,
         Department_Id = null,
         Manager_Id    = null,
         name          = null,
         state         = null,
         created_on    = null;
  update Md_Persons
     set company_id = null,
         person_id  = null,
         name       = null;
  end;

end Ui_Management123; 
```
{% endcode %}

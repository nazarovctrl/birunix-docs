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

# UIT\_\*.pck

Use UIT packages when you need to avoid writing duplicate code in UI packages that cannot be placed in module packages.

For example, if you create a function that uses a UI package internally, you cannot move this function to a module package because using a UI package inside a module package is not safe. Fortunately, you can use `uit_modulename.pck` to handle such cases.

**Example: `uit_tmd.pck`**

{% code title="Project Structure:" %}
```
📁 task_manager/
├── 📁 main/
    ├── 📁 oracle/
        ├── 📁 uit/
            ├── uit_tmd.pck
            └── <other uit packages> 
```
{% endcode %}

{% code title="uit_tmd.pck" %}
```
create or replace package Uit_Tmd is
  ----------------------------------------------------------------------------------------------------
  Procedure Department_Create
  (
    i_manager_id number := null,
    i_name       varchar2,
    i_code       number := null
  );
end Uit_Tmd;
/
create or replace package body Uit_Tmd is
  ----------------------------------------------------------------------------------------------------
  Procedure Department_Create
  (
    i_manager_id number := null,
    i_name       varchar2,
    i_code       number := null
  ) is
  begin
   Tmd_Api.Department_Save(i_company_id    => Ui.company_Id,
                          i_department_id => Tmd_Next.Department_Id,
                          i_manager_id    => i_manager_id,
                          i_name          => i_name,
                          i_state         => 'A',
                          i_code          => i_code);
  end;
end Uit_Tmd;
```
{% endcode %}

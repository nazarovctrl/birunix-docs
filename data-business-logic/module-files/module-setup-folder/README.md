---
icon: wrench
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

# Module Setup Folder

### TMD module setup folder overview

```
   ├── 📁 oracle/
        ├── 📁 module/
            ├── 📁 tmd/
                ├── 📁 setup/
                    ├── tmd_table.sql
                    ├── tmd_sequence.sql    
                    └── tmd_settings.sql 
```

Each module's setup folder contains SQL files that define the module's foundation:

### **1. Table Files**

* **Naming Pattern:** `module_name_table.sql` (e.g., `tmd_table.sql`)
* **Content:** Creates tables, comments, and indexes for the module

{% content-ref url="table.md" %}
[table.md](table.md)
{% endcontent-ref %}

### **2. Sequence Files**

* **Naming Pattern:** `module_name_sequence.sql` (e.g., `tmd_sequence.sql`)
* **Content:** Creates sequences related to the module

{% code title="tmd_sequcene.sql" %}
```sql
prompt TMD SEQUENCES
create sequence tmd_departments_sq;
```
{% endcode %}

### **3. Setting Files**

* **Naming Pattern:** `module_name_setting.sql` (e.g., `tmd_setting.sql`)
* **Content:** Configures module settings, initial data, and table settings

{% code title="tmd_settings.sql" %}
```
declare
  v_Company_Id number := Md_Pref.c_Company_Head;
begin
  z_Tmd_Departments.Insert_One(i_Company_Id    => v_Company_Id,
                              i_Department_Id => Tmd_Next.Department_Id,
                              i_name          => 'Main department',
                              i_State         => 'A',
                              i_code          => 'TMANAGER:1');
end;
```
{% endcode %}

The code creates a initial department using the Biruni Framework's standard patterns.

---
icon: table
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

# Table

### Naming and Structure

* Table names must be prefixed with the module name (e.g., `tmd_departments, tmd_work_schedules`)
* Columns follow a specific order: company\_id (if present), primary key columns, with state, code, pcode at the end if applicable

### Constraints and Indexes

* **Primary Key:** Suffix "\_pk" (e.g., `tmd_departments_pk`)
* **Foreign Key:** Suffix "\_f1", "\_f2", etc. (e.g., `tmd_departments_f1`)
* **Unique Constraint:** Suffix "\_u1", "\_u2", etc. (e.g., `tmd_departments_u1`)
* **Check Constraint:** Suffix "\_c1", "\_c2", etc. (e.g., `tmd_departments_c1`)
* **Unique Index:** Suffix "\_u1", "\_u2", etc. (continuing from unique constraints)
* **Simple Index:** Suffix "\_i1", "\_i2", etc. (e.g., `tmd_departments_i1`)

{% code fullWidth="false" %}
```sql
create table tmd_departments(
  company_id                      number(20)        not null,
  department_id                   number(20)        not null,
  manager_id                      number(20),     
  name                            varchar2(50)      not null,
  state                           varchar2(1)       not null,
  code                            number(13)        not null,
  created_on                      date,
  constraint tmd_departments_pk primary key (company_id, department_id) using index tablespace MY_INDEX,
  constraint tmd_departments_u1 unique (department_id) using index tablespace MY_INDEX,
  constraint tmd_departments_f1 foreign key (company_id, manager_id) references md_persons(company_id, person_id),
  constraint tmd_departments_c1 check (state in ('A', 'P'))
) tablespace MY_DATA;

create unique index tmd_departments_u2 on tmd_departments(company_id, code) tablespace MY_INDEX;

create index tmd_departments_i1 on tmd_departments(company_id, manager_id) tablespace MY_INDEX;
```
{% endcode %}

***

## **📦Z Package**

The **Z package** simplifies database operations by eliminating the need to write **INSERT**, **UPDATE**, **DELETE**, and **SELECT** queries manually.

### **Selecting Data Using the Z Package**

Instead of writing standard **SELECT** queries, you can use the **Z package** to retrieve data more efficiently.

**Traditional SELECT Query:**

```plsql
DECLARE
    r Tmd_Departments%ROWTYPE;
BEGIN
    -- Selecting data using a standard query
    SELECT * 
      INTO r
      FROM Tmd_Departments t
     WHERE t.Company_Id = 100
       AND t.Department_Id = 1;
END;
```

**Optimized Query Using the Z Package:**

```plsql
DECLARE
    r Tmd_Departments%ROWTYPE;
BEGIN
    -- Using the Z package to load data
    r := z_Tmd_Departments.Load(i_Company_Id => 100, i_Department_Id => 1);
END;
```

As shown above, the **Z package** simplifies queries, making code more readable and reducing the chances of errors.

***

### **Generating a Z Package**

To generate a **Z package** for a table, use the following command:

```plsql
EXEC Fazo_Z.Run('tmd_departments');
```

This will create a package named **z\_tmd\_departments** for the **tmd\_departments** table.

### **Functions & Procedures Inside a Z Package**

A generated **Z package** includes multiple functions and procedures for common operations, such as:

* **Load** – Retrieves a single record
* **Take** – Fetches data with conditions
* **Exist** – Checks if a record exists
* **Save\_Row** – Inserts or updates a record
* **Delete\_One** – Deletes a single record
* **Insert\_Row** – Inserts a new record
* **And more**

### **Generating Z Packages for Multiple Tables**

To generate **Z packages** for multiple tables:

```plsql
EXEC Fazo_Z.Run('tm'); 
```

This generates **Z packages** for all tables whose names start with **"tm"**.

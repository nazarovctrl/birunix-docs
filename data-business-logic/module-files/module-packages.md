---
icon: box-open
---

# Module Packages

```
📁 task_manager/
├── 📁 main/
    ├── 📁 oracle/
        ├── 📁 module/
            ├── 📁 tmd/
                ├── tmd_core.pck
                ├── tmd_api.pck
                ├── tmd_pref.pck
                ├── tmd_global.pck
                ├── tmd_next.pck
                ├── tmd_util.pck
                ├── tmd_watcher.pck
                ├── tmd_audit.pck
```



Module packages provide specialized functionality and follow a strict naming convention with specific purposes:

<table><thead><tr><th width="231">Package Type</th><th>Purpose</th></tr></thead><tbody><tr><td>core</td><td>Hides functions from other modules and prevents code rewriting in the API</td></tr><tr><td>api</td><td>Contains basic DML operations (insert, update, delete) used in UI</td></tr><tr><td>pref</td><td>Stores module-related constants, variables, types, settings, and getter functions</td></tr><tr><td>global</td><td>Contains global variables and types</td></tr><tr><td>next</td><td>Used to retrieve next sequence elements</td></tr><tr><td>util</td><td>Contains utility procedures and functions</td></tr><tr><td>watcher</td><td>Implements watcher logic related to module data</td></tr><tr><td>audit</td><td>Contains code for auditing module data</td></tr></tbody></table>

### **Constants and Functions Naming**

* Constants should have the prefix "c\_" (e.g., `c_Company_Head`)
* When using constants in SQL files, a wrapper function with the same name minus the "c\_" prefix should be created (e.g., `Function Company_Head`)

### **Sequence Access**

* Sequences should be accessed through the appropriate next package function
* ✅Best practice: `v_role_id := tmd_next.department_id;`
* ❌Not recommended: `v_role_id := tmd_departmetns_sq.nextval;`

### **Translation and Internationalization**

* When a module package function calls the b package's translate (`b.translate()`) function, it prefixes the module name to the first parameter.  For example:                                                                                         `b.Translate('TMD:' || i_Message, i_P1, i_P2, i_P3, i_P4, i_P5)`

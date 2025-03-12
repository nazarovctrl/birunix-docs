---
icon: suitcase
---

# Module Files

Modules in Biruni are logical groupings of files that serve a specific purpose within a project. They follow a strict hierarchy and naming convention to ensure consistency and maintainability. Biruni currently consists of 11 modules, but users can also create their own modules based on their project requirements.

## Module Naming Convention&#x20;

Modules should use short, descriptive names with a prefix indicating the project they belong to.

For example, if we have a project **Task Manager** with the prefix **TM**, the modules should be named as follows:

* **TMD (Task Manager Departments)** – Contains files related to departments.
* **TME (Task Manager Employees)** – Contains files related to employees.

<pre data-title="ms module"><code><strong>📁 task_manager/
</strong>├── 📁 main/
    ├── 📁 oracle/
        ├── 📁 module/
            ├── 📁 tmd/
                ├── 📁 setup/
                │   ├── &#x3C;Table structure>
                |   └── &#x3C;Module settings> 
                └── &#x3C;Module packages>
</code></pre>

## Existing Modules in Biruni

The following modules are pre-defined within Biruni:

<table><thead><tr><th width="148">Module name</th><th>Description</th></tr></thead><tbody><tr><td>kernel</td><td>Serves as the foundational core, managing system-wide configurations, routing, authentication, file handling, and logging for robust functionality.</td></tr><tr><td>md</td><td>Oversees core administrative data and settings, handling entities like companies, users, roles, and configurations for smooth multi-company and multi-filial operations.</td></tr><tr><td>kauth</td><td>Manages authentication and authorization, controlling IP ranges, sessions, tokens, cookies, and login attempts for secure access.</td></tr><tr><td>kl</td><td>Administers licensing operations, overseeing activations, user assignments, and usage tracking for efficient license management.</td></tr><tr><td>ms</td><td>Enhances collaboration through social and task management, supporting notifications, task tracking, chats, and SMS messaging.</td></tr><tr><td>mt</td><td>Facilitates device synchronization and logging, managing registrations, user-device bindings, and server-device data processing for mobile integration.</td></tr><tr><td>mf</td><td>Handles file operations, enabling saving, deletion, moving, copying, and sharing with users or roles for streamlined file management.</td></tr><tr><td>ker</td><td>Simplifies report generation, managing templates, user access, and role-based permissions for efficient reporting across companies and filials.</td></tr><tr><td>kdyn</td><td>Provides dynamic field management, allowing customization of entity fields and values through saving, updating, and deletion.</td></tr><tr><td>kset</td><td>Integrates Apache Superset, managing form-dashboard mappings and dataset filtering for secure, embedded analytics with row-level security.</td></tr><tr><td>bmb</td><td>Enables API integration as the Biruni Message Broker, handling endpoint configurations, request processing, and error logging for seamless service communication.</td></tr></tbody></table>

> <mark style="color:orange;">**Warning**</mark><mark style="color:orange;">: Module names not starting with 'b' will be renamed by prepending 'b' (e.g., kernel becomes bkernel, md becomes bd) to ensure naming consistency across Biruni.</mark>

### Kernel module

The kernel module provides the foundational infrastructure for Biruni, handling system operations, logging, and configuration management. It equips developers with tools to monitor daily activities and maintain system stability.

#### **Key Views Ending** in **\_today**

* **`biruni_log_today`**:
  * **Purpose**: Delivers a real-time view of system logs (biruni\_log) from the current day.
  * **Use Case**: Developers can query this view to troubleshoot recent errors or monitor system performance, accessing fields like log\_id, status, error\_message, and executed\_in without sifting through historical data.
* **`biruni_manual_log_today`**:
  * **Purpose**: Shows manual logs (biruni\_manual\_log) from today.
  * **Use Case**: Useful for debugging custom logging events triggered manually, allowing developers to focus on recent issues specific to their code or processes.
* **`biruni_app_server_exceptions_today`**:
  * **Purpose**: Provides a daily snapshot of application server exceptions (biruni\_app\_server\_exceptions) with fields like source\_class and stacktrace.
  * **Use Case**: Developers can quickly identify and resolve server-side errors from the current day, improving application stability and response time.

#### Developer Notes

* **Purpose**: Essential for daily system monitoring and diagnostics via \_today views, isolating current data for quick insights.
* **Usage**: Use these views in SQL queries to troubleshoot errors, track performance, or validate system behavior in real time.

### MD module

The md module manages Biruni’s administrative entities—companies, users, roles, permissions, and settings. It’s vital for developers creating multi-tenant, role-based applications with strong access control.

#### Md\_Api Package

This package offers procedures and functions for managing core administrative entities and their configurations.

* **`Company_Add`**
  * **Purpose**: Creates a new company record, returning its company\_id.
  * **Usage**: use to initialize a company programmatically.
* **`Company_Update`**
  * **Purpose**: Updates company details selectively (e.g., name or state).
  * **Usage**: modify company attributes as needed.
* **`Person_Save`**
  * **Purpose**: Saves or updates a person (natural/legal) with details like email or photo.
  * **Usage**: manage user profiles.
* **`User_Save`**
  * **Purpose**: Saves a user record with full attributes (e.g., login, password).
  * **Usage**: create or update users.
* **`Role_Grant`**
  * **Purpose**: Assigns a role to a user within a filial.
  * **Usage**: grant permissions to users.
* **`Form_Action_Grant`**
  * **Purpose**: Grants specific form action permissions to a role.
  * **Usage**: control UI access.
* **`Sequence_Nextval`**
  * **Purpose**: Generates the next sequence value for custom counters.
  * **Usage**: use for unique identifiers.

#### Md\_Util Package

This package provides utility functions for querying and validating administrative data.

* **`Company_Name`**
  * **Purpose**: Retrieves a company’s name by ID.
  * **Usage**: fetch company details for display.
* **`User_By_Credentials`**
  * **Purpose**: Validates and returns user details by login credentials.
  * **Usage**: authenticate users.
* **`Grant_Has`**
  * **Purpose**: Checks if a user has permission for a form action.
  * **Usage**: enforce access control.
* **`Person_Id_By_Code`**
  * **Purpose**: Finds a person’s ID by their unique code.
  * **Usage**: locate persons efficiently.
* **`Search_Form_Query`**
  * **Purpose**: Builds a query for searching forms accessible to a user.
  * **Usage**: generate dynamic search logic.

#### Developer Notes

* **Purpose**: Central to managing companies, users, roles, and permissions, enabling secure, multi-tenant applications.
* **Usage**: Use Md\_Api for entity CRUD operations (e.g., Company\_Add, User\_Save) and access control (Role\_Grant). Use Md\_Util for lookups (e.g., Company\_Name) and permission checks (e.g., Grant\_Has) in business logic.

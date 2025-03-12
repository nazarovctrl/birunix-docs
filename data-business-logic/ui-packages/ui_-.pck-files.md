---
icon: box-open
description: >-
  This page provides detailed documentation on key UI-related packages within
  the system, including session management, authentication, access control, and
  user interface utilities.
---

# UI\_\*.pck Files

## 1. UI\_AUTH.pck

This package handles authentication, session management, user login, and authorization.

_<mark style="color:blue;background-color:blue;">All authentication, session handling, and authorization are automatically managed within the framework.</mark>_

### Key Functions and Procedures:

* **`Logon_As_System`**: Grants system-level access to execute migration queries.
* **`Authenticate`**: Handles user authentication within the framework.
* **`Check_Session`**: Ensures that the current session is valid.
* **`Authorize`**: Checks whether a user has permission to perform an action.
* **`Session_Set`**: Initializes a user session with the specified parameters.
* **`Session_Close`**: Terminates an active session for a given user.
* **`Check_Route_Access`**: Verifies if a user has access to a specific route.
* **`Check_Login_Attempt`**: Ensures login attempts are within allowed limits.
* **`Increase_Login_Attempt`**: Increments the failed login attempt counter for security monitoring.

## 2. UI\_CONTEXT.pck

This package provides utility functions and procedures for managing project context, user sessions, and company information.

### Key Functions and Procedures:

* **`Project_Code`**: Retrieves the current project code.
* **`Company_Id`**: Returns the ID of the currently active company.
* **`User_Id`**: Retrieves the ID of the logged-in user.
* **`User_Name`**: Returns the name of the logged-in user.
* **`Session_Id`**: Retrieves the current session ID.
* **`Host_Id`**: Returns the host ID of the session.
* **`Lang_Code`**: Retrieves the language code for localization.
* **`Is_User_Admin`**: Checks if the current user has admin privileges.
* **`Is_Concurrent_Session_Policy_Enabled`**: Determines if multiple sessions are allowed per user.
* **`Init`**: Initializes the user session with project and branch details.
* **`Init_Migr`**: Initializes a session for database migration purposes.
* **`Set_Project_And_Filial`**: Updates the current project and branch context dynamically.
* **`Clear`**: Resets the session and context settings.

## 3. UI\_KERNEL.pck

This package provides core functionalities, including system configurations, session management, translations, notifications, file handling, and OAuth2 support.

### **Key Functions and Procedures**

* **`Is_Debug`**– Checks if the system is in debug mode.
* **`Get_Session_Info`**– Retrieves session details.
* **`Load_Filials`**– Loads available branches.
* **`Load_Menus`**– Retrieves project menus.
* **`Load_Forms`**– Loads available UI forms.
* **`Review_Log`**– Logs system events.
* **`Translate_Name`**– Translates UI messages.
* **`Load_Notifications`**– Retrieves user notifications.
* **`Load_Alerts`**– Fetches alert messages.
* **`Oauth2_Gen_Params`**– Generates OAuth2 parameters.
* **`Check_File_Limit`**– Validates file upload limits.

## 4. UI\_OAUTH2.pck

This package provides OAuth2-related procedures and functions for handling user authentication and organization mapping via **OneID** and **EIMZO**.

### **Key Functions and Procedures**

* **`Oneid_User_Save`** – Saves OneID user details.
* **`Oneid_User_Organization_Save`** – Stores user-organization mapping in OneID.
* **`Company_Oneid_User`** – Retrieves a company's OneID user record.
* **`Apply_Oneid`** – Handles OneID authentication and redirects.
* **`Apply_Eimzo`** – Handles EIMZO authentication and redirects.

## 5. UI.pck

This package provides utility functions and procedures related to user sessions, access control, UI metadata, and application context.

### **Key Functions and Procedures**

* **`Company_Id`** – Returns the current company ID of the logged-in user.
* **`User_Id`** – Retrieves the ID of the currently authenticated user.
* **`User_Name`** – Returns the name of the currently authenticated user.
* **`Session_Id`** – Retrieves the active session ID.
* **`Host_Id`** – Returns the host ID associated with the current session.
* **`Grant_Has`** – Checks if the current user has permission for a specific action.
* **`Grant_Check`** – Enforces access control, raising an error if the user lacks the necessary permissions.
* **`Request_Path`** – Retrieves the current request path.
* **`Request_Form`&#x20;**&#x20;– Returns the form associated with the current request.
* **`Request_Action_Key`** – Retrieves the action key for the current request.
* **`Menu_Name`** – Returns the name of a menu item within the specified project.
* **`Form_Name`** – Retrieves the name of a given form.
* **`User_Setting_Save`** – Saves a user-specific setting.
* **`User_Setting_Load`** – Retrieves a stored user setting, returning a default if not found.
* **`t_Active`** – Returns the localized translation for "Active".
* **`t_Passive`** – Returns the localized translation for "Passive".
* **`t_Yes`** – Returns the localized translation for "Yes".
* **`t_No`** – Returns the localized translation for "No".


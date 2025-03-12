---
icon: globe
description: >-
  This section details how the Biruni framework secures web applications using
  cookie-based authentication, ensuring a seamless and secure user experience.
---

# Web

## Login Process

#### Login Endpoint

* **Path**: `/b/biruni/s$log_in`

### Authentication Details

* Users log in with a username and password formatted as `username@companyname` (e.g., admin@duck).
  * **Username**: The user’s unique identifier (e.g., admin).
  * **Companyname**: The associated company or organization (e.g., duck).
* Upon successful login, the backend attaches a cookie to the response to maintain the user session.

## Request Example

To demonstrate how the login process works (e.g., via login.html), a **POST** request is sent to the `/b/biruni/s$log_in` endpoint with a JSON body. The **password** is provided in a **hashed** format for security.&#x20;

{% code title="request body" %}
```json
{
  "login": "admin@duck",
  "password": "e40c127f08bbf5a475019d429f281241972fc048" 
}
```
{% endcode %}

* **Note**: The password field contains a hashed value, ensuring secure transmission and storage.

## Cookie Management

### Cookie Components

* The cookie includes two key elements:
  * **JSESSIONID**: A session identifier for managing user sessions.
  * **biruni\_device\_id**: A custom identifier linked to the user’s device or session.

### Configuration Settings

* Cookie parameters are defined in the **kauth\_pref** Oracle package:
  * `c_Logged_User_Cookie_Name`: Set to biruni\_device\_id.
  * `c_Logged_User_Cookie_Expires_In`: 31104000 seconds (360 days), for long-term user tracking.
  * `c_Verified_Cookie_Expires_In`: 2592000 seconds (30 days), for verified session persistence.
  * `c_Session_Expires_In`: 1800 seconds (30 minutes), the default session duration.
* These settings allow sessions to last 30 minutes by default, with extended options for verified or logged-in users.

## Usage

* The framework automatically manages **JSESSIONID** and **biruni\_device\_id**, attaching them to each request to authenticate the user on the web application.
* No manual intervention is required; the cookies are handled seamlessly to maintain session continuity.

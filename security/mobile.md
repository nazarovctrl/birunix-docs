---
icon: mobile
---

# Mobile

This section explains how the Biruni framework secures mobile applications using token-based authentication, ensuring secure and reliable access for mobile users.

### Login Process

#### Login Endpoint

* **Path**: `/biruni/b/biruni/s:log_in_device`

#### Request Details

* The login request is a POST request with a JSON body containing device and user information.&#x20;

{% code title="request body" %}
```json
{ 
"device_name": "Oneplust8t",
"device_code": "KB2005",
"device_version": "Android 13",
"device_sdk": "Android",
"device_kind": "A",
"version_code": "13",
"version_name": "test",
"login": "admin@head",
"password_hash": "E40C127F08BBF5A475019D429F281241972FC048"
}
```
{% endcode %}

* **Login Format**: The login field follows the username@companyname format (e.g., admin@head):
  * **Username**: The user’s identifier (e.g., admin).
  * **Companyname**: The associated company (e.g., head).
* **Password Security**: The password\_hash field contains a hashed version of the user’s password to ensure secure transmission.

#### Successful Login Response

* Upon successful login, the backend returns a JSON response with user details and a token.&#x20;

{% code title="response body" %}
```json
{
"user_id": "101", 
"user_name": "Admin", 
"token": "3435755904C4C853091C30A8C97A0AEC3E7896CB2D26269B96540B98BC2D049036E77D97E58880C1EF6467016E02325A6E3A5B84C519B396EB5EA3972419BDF0",
"host_id": "21",
"company_name": "Head"
}
```
{% endcode %}

* The token is used for authenticating subsequent requests and must be included in the token header.

### Token Management

#### Configuration Settings

* Token parameters are defined in the **Kauth\_Pref** Oracle package:
  * `c_Token_Expires_In`: 7 days (default validity period).
  * `c_Token_Hard_Deadline`: 365 days (a hard limit after which the token becomes invalid).
* These settings ensure tokens remain valid for 7 days, with a maximum lifespan of 365 days.

### Usage

* After receiving the token, mobile applications must include it in the token header for all API calls to the Biruni framework.&#x20;

{% code title="Example header" %}
```
token: 3435755904C4C853091C30A8C97A0AEC3E7896CB2D26269B96540B98BC2D049036E77D97E58880C1EF6467016E02325A6E3A5B84C519B396EB5EA3972419BDF0
```
{% endcode %}

* This ensures secure authentication for all interactions with the backend.

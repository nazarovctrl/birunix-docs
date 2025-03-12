---
description: >-
  This section outlines the system level OAuth2 authorization process in the
  Biruni framework, enabling grant types such as code and password.
icon: key
---

# System level

<figure><img src="../../../.gitbook/assets/security/api-oauth2-server-clients.png" alt=""><figcaption><p>API/OAuth2 server clients form in Menu</p></figcaption></figure>

## Client Setup

### Creating a Client

* To use API tokens, a **client\_id** and **client\_secret** must first be created.

<figure><img src="../../../.gitbook/assets/security/api-oauth2-server-clients-create.png" alt=""><figcaption><p>View all created clients (<code>/biruni/kauth/client_list</code>)</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/security/api-oauth2-client-add.png" alt=""><figcaption><p>Create a client (<strong><code>/biruni/kauth/client+add)</code></strong></p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/security/api-oauth2-list.png" alt=""><figcaption><p><strong>client_id</strong> and <strong>client_secret</strong> (<code>/biruni/kauth/client_list</code></p></figcaption></figure>

* Use this form to generate unique credentials for your client.

## Token Generation

### Endpoint

* **URL**: `[server_url]/api/auth/token`

### Generating an Access Token

* External services use the **client\_id** and **client\_secret** to **generate** an **access\_token** and **refresh\_token**.

{% code title="POST request body" %}
```json
{
  "grant_type": "password",
  "client_id": "Trade client",
  "client_secret": "2D029629D607A658348AB721D3B2E747F88AB2429D1DB2E27BB2AEB5AB22CEE9A47E1A8F702A88ACB1D3193A216F1891003EDF1CFCE771A229D79C562F0E7427",
  "username": "wayne",
  "password": "no_more_pain"
}
```
{% endcode %}

* **Requirement**: grant\_type must be set to password.

### Refreshing an Access Token

* Use the **refresh\_token** to obtain a new **access\_token** without re-entering credentials.

{% code title="POST request body" %}
```json
{
  "grant_type": "refresh_token",
  "client_id": "Trade client",
  "client_secret": "698F033A41DE8B2666612B4E5FDEF518CAB20A9EB5E76633A86D83ED9175B4877985ECF27E19A252414F0B7272462136568751FDFE641D33A1E3DA8827249A6F",
  "refresh_token": "4ABBBA0F2AFFCFD18495798CC044C57230D2E9069053F31434C3DFDA8A4C442CEB858E441B60D4CD5B9DDC8025C3D814CA11A7AED59D2D2854A7047C09D2386B"
}
```
{% endcode %}

* **Requirement**: **grant\_type** must be set to **refresh\_token**.

### Token Response

* The response for both token generation and refresh is a JSON object

```json
{
  "token_type": "Bearer",
  "access_token": "25ADCD385B01B005FED2786DFCB33B7D316E84A912A714E619E08205FC7ADFA62276EC243EDE7245D4F38640CA714C828A6E71C969732C85658EF4E90239EAB7",
  "refresh_token": "4ABBBA0F2AFFCFD18495798CC044C57230D2E9069053F31434C3DFDA8A4C442CEB858E441B60D4CD5B9DDC8025C3D814CA11A7AED59D2D2854A7047C09D2386B",
  "expires_in": 10800
}
```

#### **Fields**:

* **token\_type**: Always Bearer.
* **access\_token**: Used for API authentication.
* **refresh\_token**: Used to request a new access\_token.
* **expires\_in**: Duration in seconds (e.g., 10800 seconds = 3 hours) before the access\_token expires.

## Usage

* Include the **access\_token** in the **Authorization** header of API requests (e.g., Authorization: Bearer \<access\_token>).
* Use the **refresh\_token** to generate a new **access\_token** when the current one expires.

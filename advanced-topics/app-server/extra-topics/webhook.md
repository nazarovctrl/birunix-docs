---
icon: webhook
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

# Webhook

### Introduction

Webhooks are user-defined HTTP callbacks that are triggered by specific events in a web application. They allow different systems to communicate with each other in real-time by sending automated messages or data updates from one system to another whenever a particular event occurs. Webhooks are commonly used in various applications, such as chatbots, payment gateways, and social media platforms, to provide real-time notifications and updates to users.

### How Webhooks Work within biruni/xcore Framework

New **KWH** module added to xcore project which is responsible for handling webhook events. It allows developers to register webhook events and their handlers in a centralized location, making it easier to manage and maintain webhook configurations.

#### Main Tables

1. **kwh\_events** table: Stores information about webhook events, such as the event code, description.
2. **kwh\_registers** table: Stores information about webhook event registrations. Each company has its own set of webhook event registrations, which are used to determine which events should trigger a webhook callback.
3. **kwh\_hooks** table: Stores information about generated webhook hooks. When events are triggered, the system generates a webhook hook for each registered event, which contains the event data. Application server watches this table for new hooks and sends them to the webhook URL. Once the hook is sent, it is marked as sent in the table. If the webhook fails, the system will retry sending the hook overall 7 times within a 24-hour period, and if it still fails, the hook will be marked as failed.
4. **kwh\_hook\_logs** table: Stores information about webhook event logs. Each time application server sends a webhook hook, it logs the event in this table with response data, whether it was successful or failed.

#### Webhook Events

Developers should write a **events** and **settings.sql** file which inserts the events into the **kwh\_events** table. Then trigger functions _**(uit\_kwh.add\_hook(event\_code, data) or kwh\_core(event\_core, company\_id, data))**_ should be written in suitable places to generate webhook hooks. E.g. when a new order is created, a trigger function should be written at the end of the crete\_order procedure to generate a hook for the **ORDER\_CREATED** event.

#### Webhook Registration

Each company should register the webhook events they want to listen to. See _**/core/kwh/register\_list**_ form. If registered url doesn't receive the hook within 2 days, the registration state will be automatically change to **PASSIVE**.

#### Webhook Handler

Application server listens to the **kwh\_hooks** table for new hooks and sends them to the webhook URL. To enable this **application.properties** file should be configured. When a webhook event is triggered, the system sends a POST request to the webhook URL with the event data.

```properties
core.webhook.enabled=false
```

#### Webhook data format

```javascript
{
  "type": "event_code",
  "timestamp": 1716288916572, // hook created time in millis
  "data": {
    "id": 1, // hook id, unique identifier
    ... // rest of the data
  }
}
```

#### Webhook headers

The system sends the following headers with each webhook request:

* **Content-Type**: application/json
* **webhook-id**: unique identifier of the hook
* **webhook-timestamp**: hook created time in milliseconds
* **webhook-signature**: hash signature of the webhook, it will be sent only if the webhook URL requires HMAC(sha256) authentication or Asymmetric encryption(ed25519).

#### Webhook authentication methods

There a five ways to authenticate webhook requests is implemented in the system:

* **No authentication**: Anyone can send a request to the webhook URL.
* **Basic authentication**: The webhook URL requires a username and password.
* **Token-based authentication**: The webhook URL requires a **token** in the request header.
* **HMAC(sha256) authentication**: The webhook URL requires a **webhook-signature** in the request header.
* **Asymmetric encryption(ed25519)**: The webhook URL requires a **webhook-signature** in the request header.

Webhook signature is signed in format of **{hook\_id}.{timestamp}.{payload}**. The receiver system should verify the signature by using the same algorithm and secret key.

In **HMAC(sha256)** authentication method signature header format is **v1,{signature}**. **{signature}** is **sha256Encryption({hook\_id}.{timestamp}.{payload})**.

In **Asymmetric encryption(ed25519)** authentication method signature header format is **v1a,{signature}**. **{signature}** is **ed25519Encryption({hook\_id}.{timestamp}.{payload})**.

For additonal information visit [standard-webhooks viki page](https://github.com/standard-webhooks/standard-webhooks/blob/main/spec/standard-webhooks.md)

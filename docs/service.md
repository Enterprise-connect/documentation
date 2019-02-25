# Service
This page is written to help people understand the Enterprise Connect (EC) Service.

* [Basics](#basics)
* [APIs](#apis)

---
---

## Basics
Behind the scenes, the EC Service is a Node app. When a user subscribes to EC in Predix, something we refer to as a 'service instance app' is deployed in our team's relevant production space in Predix (different regional datacenters are logically separated). It is named using the GUID of the Service as it appears in the user's space (if the user runs `cf service SERVICE_NAME --guid`, the value returned is used for the service instance app name).

It's not the Gateway! The most common misconception is that the EC Service and the EC Agent Gateway are somehow the same entity, but this is not even remotely true. One is a single-tenant Node application in Cloud Foundry, and the other is a one-size-fits-most, compiled GoLang binary.

It's just a backend. At the end of the day, the EC Service is simply a series of APIs that the EC Agents use for various interactions and authorizations between themselves and other entities.

## APIs
The APIs available can be found by visiting the Service URI that is found in the [EC Service credentials](./service-credentials.md). They have Swagger support, but are also explained below.

[back to top](#service)
# Service

(WIP)

This page is written to help people understand the Enterprise Connect (EC) Service.

* [Basics](#basics)
* [APIs](#apis)

![EC Service APIs](../images/serviceApisCollage.png) 

---
---

## Basics
Behind the scenes, the EC Service is a Node app. When a user subscribes to EC in Predix, something we refer to as a 'service instance app' is deployed in our team's relevant production space in Predix (different regional datacenters are logically separated). It is named using the GUID of the Service as it appears in the user's space (if the user runs `cf service SERVICE_NAME --guid`, the value returned is used for the service instance app name).

**It's not the Gateway.** The most common misconception is that the EC Service and the EC Agent Gateway are somehow the same entity, but this is not even remotely true. One is a single-tenant Node application in Cloud Foundry, and the other is a one-size-fits-most, compiled GoLang binary.

**It's just a backend.** At the end of the day, the EC Service is simply a series of APIs that the EC Agents use for various interactions and authorizations between themselves and other entities.

## APIs
The APIs available can be found by visiting the Service URI that is found in the [EC Service credentials](./service-credentials.md). They have Swagger support, but are also explained below.

* [Credentials](#credentials)
* [/accounts]

### Credentials
First and foremost, to use the Service APIs, you should understand the credentials needed, and how they are used. Most of the time, users will likely just need the [admin token](./service-credentials.md#understanding-credentials) for their EC Service, but sometimes may need the [UAA Client ID and Secret](./uaa.md#client-creation) to fetch a bearer token.

#### Username:Password
When you visit the EC Service URI in a browser, you will be prompted for a username and password associated with the Service. To obtain these values, you must 'decode' the [admin token](./service-credentials.md#understanding-credentials) for their EC Service. There are several ways to do this, here are a few ideas:

- Website
	- https://www.base64decode.org/

- Shell
	- `echo YWRtaW46cGFzc3dvcmQ= | base64 --decode` (using your admin token, this is an example value)

- Using Chrome Inspect/Console
	- Right click the page in Chrome
	- Click 'Inspect'
	- Near the top of the field that should pop out are several icons and tabs, find and click the Console tab
	- In the console tab, enter: `atob('YWRtaW46cGFzc3dvcmQ=')` (using your admin token, this is an example value)

#### Basic Token


#### Bearer Token



[back to top](#service)

[documentation home](https://enterprise-connect.github.io/documentation/) 
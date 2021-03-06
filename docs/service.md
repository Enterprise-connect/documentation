# Service

This page is written to help people understand the Enterprise Connect (EC) Service.

* [Basics](#basics)
* [IDs](#ids)
* [Groups](#groups)
* [APIs](#apis)

![EC Service APIs](../images/serviceApisCollage.png) 

---
---

## Basics
Behind the scenes, the EC Service is a Node app. When a user subscribes to EC in Predix, something we refer to as a 'service instance app' is deployed in our team's relevant production space in Predix (different regional datacenters are logically separated). It is named using the GUID of the Service as it appears in the user's space (if the user runs `cf service SERVICE_NAME --guid`, the value returned is used for the service instance app name).

**It's not the Gateway.** The most common misconception is that the EC Service and the EC Agent Gateway are somehow the same entity, but this is not even remotely true. One is a single-tenant Node application in Cloud Foundry, and the other is a one-size-fits-most, compiled GoLang binary.

**It's just a backend.** At the end of the day, the EC Service is simply a series of APIs that the EC Agents use for various interactions and authorizations between themselves and other entities.

## IDs
IDs are the 'bread and butter' of EC Agent Client and EC Agent Server communications. One of the first things the EC Agent Gateway does is to verify if the requesting EC Agent Client's ID and the targeted EC Agent Server's ID are part of the same [group](#groups). If they are not part of the same group, even if they are valid for the EC Service otherwise, communication will fail.

> With the [Accounts APIs](#accounts), an ID from one group can be added to another, making it valid for both groups! This is not hugely important, but is another way of organizing and managing use-cases, users, developers, teams, etc.

## Groups
Enterprise Connect uses the concept of 'groups' for both security management, as well as a tool for organization. When a new Service is created, it will come with a single group, which shares a name with the ['zone'](./service-credentials.md#understanding-credentials) of the Service, and that group will contain the two randomly generated IDs for use with the first EC Agent Server and EC Agent Client.

### Groups and IDs
The [Accounts APIs](#accounts) provide ways to manage and understand these groups and IDs. Using the functionality of groups is not at all required. You are free to add more IDs to the default group. With that said, the utility of groups and IDs can be as robust as you would like to make it. An admin-person is able to 'secure' the APIs behind additional auth requirements, and then can propagate desired group/ID information to appropriate parties. 

> **Why bother?** Because EC is incredibly touchy (obedient and un-thinking), and the vast majority of users only spend enough time learning EC to get the initial config working, many are unaware that a single uninformed person can crash all connectivity for an entire EC Service. If everyone on the team has access to everything - when things go wrong, it is very hard to find out who did what/why/how/when. By leveraging groups, IDs, whitelists, blocklists, etc., admin personas can ensure they have an airtight grip on their EC Service.


## APIs
The APIs available can be found by visiting the Service URI that is found in the [EC Service credentials](./service-credentials.md). They have Swagger support, but are also explained below.

* [Accounts](#accounts)

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
	- You should see the results in double-quotes directly afterwards, i.e. `"admin:password"`

#### Basic Token
The basic token is used as authorization on the most common, and user-relevant endpoints. When using the basic token, be sure to populate the appropriate field with the word 'basic', a space, and then your token. You should have something like:

> basic YWRtaW46cGFzc3dvcmQ=

Instructions on how to populate other fields provided in subsequent sections.

#### Bearer Token
Similar to using the basic token, authorization fields must provide the word 'bearer', a space, and then a valid (non-expired) bearer token:

> bearer eyJhbG...

---
---

## Accounts
Below you will find an explanation of all the endpoint available in the 'Accounts' API family. When you visit the Service URI in your browser, you will see we have ['Swagger'](https://swagger.io/) support which explains the usage there as well. **We recommend allowing 24 hours to elapse before using any groups or IDs generated in a production environment**

* [Generate Additional IDs](#post-adminaccountsgroup-idadd)
* [Create a Group](#post-adminaccountsgroup-id)
* [Check a Group's IDs and Settings](#get-adminaccountsgroup-id)
* [List All Groups](#get-adminaccountslist)

### POST /admin/accounts/{group-id}/add 
> Generate an EC system account

---

Here you can add a new, randomly generated ID to the specified group. Here is an example, valid `curl` command for reference:

*for default group...*
```bash
curl -X POST \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: basic YWRtaW46cGFzc3dvcmQ=' \
'https://4988a094-66af-47ff-bbdd-d894870272bf.run.aws-usw02-pr.ice.predix.io/v1/admin/accounts/4988a094-66af-47ff-bbdd-d894870272bf/add'
```

*for custom group...* (notice it's nearly the same!)
```bash
curl -X POST \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: basic YWRtaW46cGFzc3dvcmQ=' \
'https://4988a094-66af-47ff-bbdd-d894870272bf.run.aws-usw02-pr.ice.predix.io/v1/admin/accounts/some-custom-group/add'
```

*example response/output*
```json
{
  "status": "account generated/added.",
  "details": {
    "ids": [
      "evV6ZY",
      "DZok1N",
      "tdHFkb"
    ],
    "trustedIssuerIds": [
      "https://cfa4ec8d-7d9e-4dca-b318-5f67b98afe14.predix-uaa.run.aws-usw02-dev.ice.predix.io/oauth/token"
    ]
  }
}
```

[back to APIs](#apis)

[back to top](#service)

---
---

### POST /admin/accounts/{group-id}/add/{agent-id} 
> Attach an existing agent id to an exiting EC system account

---

~~Here you can make an ID present in one group, available in another.~~

This API is currently behaving badly.

<!-- *example request*
```bash
curl -X POST \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: basic YWRtaW46cGFzc3dvcmQ=' \
'https://4988a094-66af-47ff-bbdd-d894870272bf.run.aws-usw02-pr.ice.predix.io/v1/admin/accounts/some-custom-group/add/evV6ZY'
```

*example response/output*
```javascript
{
  "status": "account generated/added.",
  "details": {
    "ids": [
      "nBpxA6",
      "F5GCT1",
      "evV6ZY" // the ID from our default group,now present in our 'some-custom-group' 
    ]
  }
}
``` -->

[back to APIs](#apis)

[back to top](#service)

---
---

### DELETE /admin/accounts/{group-id} 
> Delete the EC system account

---

You can use this to delete a group and all of its IDs. You cannot delete the default group.


*example request*
```bash
curl -X DELETE \
--header 'Accept: application/json' \
--header 'Authorization: basic YWRtaW46cGFzc3dvcmQ=' \
'https://4988a094-66af-47ff-bbdd-d894870272bf.run.aws-usw02-pr.ice.predix.io/v1/admin/accounts/some-custom-group'
```

*example response/output* **don't worry this is normal**
```json
{
  "status": "failed authentication"
}
```

**It's normal to get a 404 and 'failed authentication' message. You can try performing a GET on the deleted group to 'prove' it's gone, or hit the /list API to see that it's not included there**

[back to APIs](#apis)

[back to top](#service)

---
---

### GET /admin/accounts/{group-id} 
> Get the EC system account

---

Here you can see the settings for a particular group.

*example request*
```bash
curl \
--header 'Accept: application/json' \
--header 'Authorization: basic YWRtaW46cGFzc3dvcmQ=' \
'https://4988a094-66af-47ff-bbdd-d894870272bf.run.aws-usw02-pr.ice.predix.io/v1/admin/accounts/4988a094-66af-47ff-bbdd-d894870272bf'
```

*example response/output*
```json
{
  "ids": [
    "3zOjfn",
    "0iAkp5"
  ],
  "trustedIssuerIds": [
    "https://cfa4ec8d-7d9e-4dca-b318-5f67b98afe14.predix-uaa.run.aws-usw02-dev.ice.predix.io/oauth/token"
  ]
}
```

[back to APIs](#apis)

[back to top](#service)

---
---

### POST /admin/accounts/{group-id} 
> Create a pair of EC system accounts and assigned to the indicated group.

---

Here you can create a new group with a name of your choosing, the group will come with two randomly generated IDs for use with an initial EC Agent Client and EC Agent Server.

*example request*
```bash
curl -X POST \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: basic YWRtaW46cGFzc3dvcmQ=' \
'https://4988a094-66af-47ff-bbdd-d894870272bf.run.aws-usw02-pr.ice.predix.io/v1/admin/accounts/some-custom-group'
```

*example response/output*
```json
{
  "status": "account group created."
}
```

[back to APIs](#apis)

[back to top](#service)

---
---

### PUT /admin/accounts/{group-id} 
> Update the EC Service settings in the account

---

Here you can update the trustedIssuerId of the EC Service to use a different UAA.

[back to APIs](#apis)

[back to top](#service)

---
---

### GET /admin/accounts/list 
> Get the list of account available for agent Ids

---

Here you can get a list of all the groups for a particular EC Service.

*example request*
```bash
curl \
--header 'Accept: application/json' \
--header 'Authorization: basic YWRtaW46cGFzc3dvcmQ=' \
--header 'predix-zone-id: 4988a094-66af-47ff-bbdd-d894870272bf' \
'https://4988a094-66af-47ff-bbdd-d894870272bf.run.aws-usw02-pr.ice.predix.io/v1/admin/accounts/list'
```

*example response/output*
```json
{
  "groups": [
    "4988a094-66af-47ff-bbdd-d894870272bf",
    "some-custom-group"
  ]
}
```

[back to APIs](#apis)

[back to top](#service)

---
---

### GET /admin/accounts/validate 
> Validate the agent ids if both are in a same group

---

Here you can check if two IDs belong to the same group. It will return the name of the group they belong to and a 200 response code if the IDs are validated.

*example request*
```bash
curl \
--header 'Accept: application/json' \
--header 'Authorization: basic YWRtaW46cGFzc3dvcmQ=' \
--header 'predix-zone-id: 4988a094-66af-47ff-bbdd-d894870272bf' \
'https://4988a094-66af-47ff-bbdd-d894870272bf.run.aws-usw02-pr.ice.predix.io/v1/admin/accounts/validate?agtId1=3zOjfn&agtId2=0iAkp5'
```

*example response/output*
```json
{
  "groupIds": [
    "4988a094-66af-47ff-bbdd-d894870272bf"
  ]
}
```

[back to APIs](#apis)

[back to top](#service)

---
---

### Pro Tips
- You can import curl statements to Postman, and export Postman 'code' as a curl command (or Java library style, JS library style, Python library style, etc.)! A very handy tool for those learning or new to developing web-apps that have or interact with REST APIs

[back to top](#service)

[documentation home](https://enterprise-connect.github.io/documentation/) 
# Service Credentials
This page aims to help users understand Enterprise Connect ('EC') credentials, how to access them, and where to use them.

* [Accessing Credentials](#accessing-credentials)
* [How-To](#how-to)

---
---

## Accessing Credentials
There are a couple of methods readily available to users in order to obtain credentials for an EC Service. We strongly recommend the use of [service keys](#service-key) for all users, especially those new to Cloud Foundry, as this adds the least cognitive overhead to an already esoteric process.

* [Service Key](#service-key)
* [Application Binding](#application-binding)

### Service-Key
Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to create a Service Key:

```
cf create-service-key <EC Service name> <a name for the Service Key>
```

After creating a Service Key, use this command to view the EC credentials:

```
cf service-key <EC Service name> <Service Key name>
```

This should return something along these lines (ENVs/'VCAP'):

JSON NOTATION

```json
{
 "ec-info": {
  "adm_tkn": "YWRtaW46c2VjcmV0",
  "ids": [
   "q1w2e3",
   "r4t5y6"
  ],
  "trustedIssuerIds": [
   "https://my-predix-uaa-guid.predix-uaa.run.aws-usw02-pr.ice.predix.io/oauth/token"
  ]
 },
 "service-uri": "https://12345678-abcd-1234-abcd-12345678abcd.run.aws-usw02-pr.ice.predix.io/v1beta/index/",
 "usage-doc": "https://github.com/Enterprise-connect/documentation",
 "zone": {
  "http-header-name": "Predix-Zone-Id",
  "http-header-value": "12345678-abcd-1234-abcd-12345678abcd",
  "oauth-scope": "enterprise-connect.zones.12345678-abcd-1234-abcd-12345678abcd.user"
 }
}
```

JS NOTATION

```javascript
{
 "ec-info": {
  "adm_tkn": "YWRtaW46c2VjcmV0",
  "ids": [
   "q1w2e3",
   "r4t5y6"
  ],
  "trustedIssuerIds": [
   "https://my-predix-uaa-guid.predix-uaa.run.aws-usw02-pr.ice.predix.io/oauth/token"
  ]
 },
 "service-uri": "https://12345678-abcd-1234-abcd-12345678abcd.run.aws-usw02-pr.ice.predix.io/v1beta/index/",
 "usage-doc": "https://github.com/Enterprise-connect/documentation",
 "zone": {
  "http-header-name": "Predix-Zone-Id",
  "http-header-value": "12345678-abcd-1234-abcd-12345678abcd",
  "oauth-scope": "enterprise-connect.zones.12345678-abcd-1234-abcd-12345678abcd.user"
 }
}
```

GENERIC CODE BLOCK

```
{
 "ec-info": {
  "adm_tkn": "YWRtaW46c2VjcmV0",
  "ids": [
   "q1w2e3",
   "r4t5y6"
  ],
  "trustedIssuerIds": [
   "https://my-predix-uaa-guid.predix-uaa.run.aws-usw02-pr.ice.predix.io/oauth/token"
  ]
 },
 "service-uri": "https://12345678-abcd-1234-abcd-12345678abcd.run.aws-usw02-pr.ice.predix.io/v1beta/index/",
 "usage-doc": "https://github.com/Enterprise-connect/documentation",
 "zone": {
  "http-header-name": "Predix-Zone-Id",
  "http-header-value": "12345678-abcd-1234-abcd-12345678abcd",
  "oauth-scope": "enterprise-connect.zones.12345678-abcd-1234-abcd-12345678abcd.user"
 }
}
```

[back to top](#service-credentials)
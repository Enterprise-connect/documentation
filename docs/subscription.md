# Subscription
This page provides all information required to understand the subscription process for Enterprise Connect on Predix.

* [Requirements](#requirements)
* [How-To](#how-to)

## Requirements
The Enterprise Connect Service requires a valid [UAA Instance](https://www.predix.io/services/service.html?id=1172) on Predix. Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to view info about the Enterprise Connect Service: 
 
```bash
cf m -s enterprise-connect
```
 
Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to create an EC Service Instance in your org and space: 
 
```bash
cf create-service enterprise-connect Oneway-TLS <name the service> -c <trusted issuer json, see below>
``` 
 
**Format the trusted issuer JSON as follows:**

```javascript
{"trustedIssuerIds":["https://<UAA URL>/oauth/token"]}
```

This can get unduly tricky with escape characters. If you are working out of a relevant directory, i.e., 'my-enterprise-connect-configuration/', you can just save the trusted issuer as a JSON file, and then simply reference that file in the CF create-service command.
```bash
cf create-service enterprise-connect Oneway-TLS <name the service> -c uaa.json
```

### Pro Tip
- We have an [example json](../reference/uaa.json) that makes this easier, just swap out the nonsense with your UAA details



## How-To

## The Enterprise Connect Service requires a valid [UAA Instance](https://www.predix.io/services/service.html?id=1172) on Predix
Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to view info about the Enterprise Connect Service:
```bash
cf m -s enterprise-connect
```
Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to create an EC Service Instance in your org and space:
```bash
cf create-service enterprise-connect Oneway-TLS <name the service> -c <trusted issuer json, see below>
```
**Format the trusted issuer JSON as follows:**
```javascript
{"trustedIssuerIds":["https://<UAA URL>/oauth/token"]}
```
This can get unduly tricky with escape characters. If you are working out of a relevant directory, i.e., 'my-enterprise-connect-configuration/', you can just save the trusted issuer as a JSON file, and then simply reference that file in the CF create-service command.
```bash
cf create-service enterprise-connect Oneway-TLS <name the service> -c oauth.json
```

### Get the EC Service Credentials via 'Service Key'
Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to create a Service Key:
```
cf create-service-key <EC Service name> <a name for the Service Key>
```
After creating a Service Key, use this command to view the EC credentials:
```
cf service-key <EC Service name> <Service Key name>
```

This should return something along these lines (ENVs/'VCAP'):

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
Please copy and paste this information to a document you can readily access for upcoming configurations, preferably in a working directory holding all documents related to this setup and configuration.
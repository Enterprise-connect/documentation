# Subscription
This page provides all information required to understand the subscription process for Enterprise Connect on Predix.

* [Requirements](#requirements)
* [How-To](#how-to)
* [Pro Tips](#pro-tips)

---
---

## Requirements

* [Predix Org/Space Access](https://docs.cloudfoundry.org/concepts/roles.html)
* [Predix UAA](https://www.predix.io/services/service.html?id=1172)

Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to view info about the Enterprise Connect Service: 
 
```bash
cf m -s enterprise-connect
```
 
### Pro Tips
- Shared-UAA works, but we have seen some teams struggle with delays stemming from a central admin approach
- While the first step(s) can be performed via web-app/UI, it is *stongly recommended* users begin using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html) as soon as possible, as the staggering bulk of EC operations require this

## How-To

Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to create an EC Service Instance in your org and space: 

```
cf create-service enterprise-connect Oneway-TLS <name the service> -c <trusted issuer json, see below>
``` 

**Format the trusted issuer JSON as follows:** 

```
{"trustedIssuerIds":["https://<UAA URL>/oauth/token"]}
```

This can get unduly tricky with escape characters. If you are working out of a relevant directory, i.e., 'my-enterprise-connect-configuration/', you can just save the trusted issuer as a JSON file, and then simply reference that file in the CF create-service command. 

```
cf create-service enterprise-connect Oneway-TLS <name the service> -c uaa.json
``` 


## Next Steps
* [Retrieving Service Credentials](./service-credentials.md)
* [Configuring UAA Client](./uaa.md)

### Pro Tips
- We have an [example json](../reference/uaa.json) that makes this easier, just swap out the nonsense with your UAA details

[back to top](#subscription)

[documentation home](https://enterprise-connect.github.io/documentation/) 
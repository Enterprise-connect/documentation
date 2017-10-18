# Comprehensive Guide to Enterprise Connect

* [Service Creation](#service-creation)
* [UAA Client Update](#uaa-client-update)
* [Script Templates](#script-templates) 
* [Pushing Agents](#pushing-agents) 
* [FAQs](#faqs) 
* [Common Problems and Resolutions](#common-problems-and-resolutions) 

## Service Creation
#### The Enterprise Connect Service requires a valid UAA Instance on Predix
Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to view Services and Plans in CloudFoundry Marketplace:
```
cf m
```
Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to create an EC Service Instance in your org and space:
```
cf create-service enterprise-connect <plan> <service instance name> -c <trusted issuer as a json or path to a json file>
```
The format for the trustedIssuerId JSON should be as follows:
```javascript
{"trustedIssuerIds":["https://<UAA URL>/oauth/token"]}
```
For best results, save the JSON to a file, and perform the EC creation command from that directory. The raw JSON may require escape characters if entered directly to the command line. Using a .json file circumvents this issue.</br></br>
#### Bind the EC Service to an App
To gain access to the important credentials related to your newly created service, EC must be bound to an app in your org/space. The app you choose is irrelevant. Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to bind the EC Service to an app of your choice:
```
cf bind-service <app name> <EC Service name>
```
Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to view the important credentials related to your service:
```
cf env <name of app you bound the EC Service to>
```
Navigate the JSON that is returned (later referred to as a 'VCAP') and find the portion reflecting the name of your EC Service. It is recommended that you copy everything starting from the word "credentials", and then paste this in a text document for reference, which will prove to be an invaluable time-saver while configuring your EC Agent scripts. Documenting this information is also crucial in regards to knowledge transfers.</br></br>
## UAA Client Update
#### After the creation of the EC Service, a UAA Client must be provisioned and properly updated 
- 'Authorized Grant Types' must be updated to include 'client_credentials' and 'refresh_token'
- The name of the UAA Client, as well as the UAA Client 'secret', will be needed in configuring EC agent scripts
- Find 'oauth-scope' in the EC portion of the VCAP, and add this to the 'authorities' (not scope!) of the UAA Client 
- Take note of the 'Token Validity' for your UAA Client, this will also be important in EC agent configuration
## Script Templates 
- Support Secured Websocket Connection (SSL/TLS).
- Support Corporate Proxy Services.
- Support Multi-tenancy (Multi-Servers/Clients).
- Support All Streaming/non-streaming TCP protocols. (ICMP, SSH, FTP, sFTP, etc.)
- Supports Bi-Directional streaming provided by the Web Socket Protocol.
- Support Client => Gateway <= Server model. (new)
- Support Server-side Connectivity monitoring (wakeup request)
- Support Basic/UAA/OAuth/OAuth2 Authentication.
- Support Whitelist/Blocklist configuration.
- IPv4/IPv6 IP filtering.
- RFC 5766 (Turn Service) implemented.

## Pushing Agents
- Client/Server CLI.
- Gateway CLI.
- [Java RS Client Library](https://github.build.ge.com/212359746/phConnectivityJavaClientLib)
- Gateway as a Service/Tile in Predix CF.
- UDP/Datagram Socket implementation.
- RAAS Load-Balancer module.

## FAQs


## Common Problems and Resolutions
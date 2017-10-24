# Comprehensive Guide to Enterprise Connect

* [Service Creation](#service-creation)
* [UAA Client Update](#uaa-client-update)
* [Script Templates](#script-templates) 
* [Pushing Agents to Predix](#pushing-agents-to-predix) 
* [Diego, Scaling, and Managing Complex Use Cases](#diego-scaling-and-managing-complex-use-cases)
* [FAQs](#faqs) 
* [Common Problems and Resolutions](#common-problems-and-resolutions) 
* [References and Further Resources](#references-and-further-resources)

## Service Creation
#### The Enterprise Connect Service requires a valid [UAA Instance](https://www.predix.io/services/service.html?id=1172) on Predix
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
## [UAA Client](https://predix-toolkit.run.aws-usw02-pr.ice.predix.io/) Update
#### After the creation of the EC Service, a [UAA Client](https://predix-toolkit.run.aws-usw02-pr.ice.predix.io/) must be provisioned and properly updated 
- 'Authorized Grant Types' must be updated to include 'client_credentials' and 'refresh_token'
- The name of the UAA Client, as well as the UAA Client 'secret', will be needed in configuring EC agent scripts
- Find 'oauth-scope' in the EC portion of the VCAP, and add this to the 'authorities' (not scope!) of the UAA Client 
- Take note of the 'Token Validity' for your UAA Client, this will also be important in EC agent configuration
</br></br>
## Script Templates
#### For best results please use the following templates to configure your EC agent scripts:
##### EC Gateway Agent
```bash
./ecagent_linux_sys -mod gateway -lpt ${PORT} -zon <Predix-Zone-ID> -sst <EC-Service-URI> -tkn <admin-token> -dbg
```
Agents running on Predix will always require the Linux agent binary, but other agents will require the appropriate binary based on the environment for your use case.
##### EC Server Agent
```bash
./ecagent_OS_Version -mod server -aid <VCAP_provided> -cid <UAA_client_name> -csc <UAA_client_Secret> -dur 1200 -zon <Predix-Zone-ID> -sst <EC-Service-URI> -hst wss://<Predix_Gateway_App_URL>/agent -oa2 https://<predixUAA_URL>/oauth/token -rht <IP of data source> -rpt 5432 -dbg -hca ${PORT}
```
'${PORT}' will cause Predix to dynamically assign an available port. If ran elsewhere, '${PORT}' will need to be replaced with a port of your choice, which is not in use. Be sure the '-dur' flag used, which represents how often the agent will fetch a new token from the UAA in minutes, is lower/shorter than the 'Token Validity' values on your UAA Client (the agents need to refresh tokens before the UAA Client expires them).
##### EC Client Agent
```bash
./ecagent_OS_Version -mod client -aid <VCAP_provided> -tid <EC Server Agent '-aid'> -cid <UAA_client_name> -csc <UAA_client_Secret> -dur 1200 -hst wss://<Predix_Gateway_App_URL>/agent -oa2 https://<predixUAA_URL>/oauth/token -lpt <Defined_by_You> -dbg
```
Agents not running on Predix may require an additional proxy flag. You will need to identify what proxy is appropriate for your environment, and then add this flag to the end of the script:
```bash
-pxy <your proxy, no passwords allowed>
```
</br>

## Pushing Agents to Predix
### The following instructions are absolutely critical to overall connectivity and behavior of the agents on Predix:
- You will need at least <a href="https://github.com/Enterprise-connect/ec-agent-cf-push-sample/tree/dist" target="_blank">three items present</a> to properly push an EC agent to Predix:
    1. a [file](https://github.com/Enterprise-connect/ec-agent-cf-push-sample/blob/dist/ec.sh) to start the agent binary with agent-mode specific flags, commonly named 'ec.sh'
    2. the Linux [binary](https://github.com/Enterprise-connect/ec-sdk/blob/dist/dist/ecagent_linux_sys.tar.gz)
    3. a [manifest.yml](https://github.com/Enterprise-connect/ec-agent-cf-push-sample/blob/dist/manifest.yml)
        - you will need to update the 'name:' field of the manifest to push your app, unless you choose to override that via command line
- You will need to [add and install the Diego CF CLI plug-in](https://github.com/cloudfoundry-incubator/Diego-Enabler) with the commands found under installation
    - Run both commands, regardless of any perceived error after the first
#### Copy, paste, update, and utilize the following commands from the directory of your ec.sh, agent binary, and manifest.yml to push your app to predix
***Caution!*** If you are re-pushing an existing EC agent app, it is advised you begin with this command:
```bash
cf d -r -f <app name>
```
For a fresh app, or after you have deleted the previous app, use the following:
```bash
cf push --no-route 
cf enable-diego <app name>
cf map-route <app name> <run.your.domain.predix.io> -n <app/route name>
```
You now have access to powerful features such as scaling, allowing you to push a single Gateway app, and then scale it up - each Gateway instance able to handle 50 concurrent sessions! In fact, the EC team considers it best practice to scale your Gateway up to at least two instances:
```bash
cf scale <Gateway app name> -i 2
```
## Diego, Scaling, and Managing Complex Use Cases
### Diego-enabled Agent Apps on Predix and Scaling
With the introduction, and requirement, of the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html) [Diego plugin](https://github.com/cloudfoundry-incubator/Diego-Enabler) for all EC Agents running on Predix, a powerful feature has been established. This update has provided our users the ability to *scale* their EC Agents running on Predix (this can also be mimicked locally and manually) with a simple command:
```bash
cf scale <app name> -i <number of instances you want to scale to>
```
Users no longer need to be concerned with traffic to a Gateway requiring additional Gateways, Servers, Clients, etc. Each Gateway instance will provide *load-balanced* support for up to 50 concurrent sessions! The Gateway will employ machine learning to ensure that the traffic will be normalized across all instances of the Gateway. In other words, if you were to see 10 sessions on one instance of the Gateway, you should also see a number of sessions quite near that on the others.
### EC Usage with Multiple Data Sources and Client-side Applications
When you create an EC Service instance, you are provided with two IDs by default. These IDs are used to configure your Server and Client scripts, as discussed previously. This is only going to be adequate for very basic use cases and POCs. Many users new to EC are unaware of the expansive toolkit available at the Service URI. If you navigate to your Service URI, you can click 'API Docs' on the left nav-bar, at which point you will be prompted for a username and password. To obtain your credentials, find your admin token(*adm_tkn*) on your Service's VCAP, and [decode](https://www.base64decode.org/) this to view your credentials. For example:
> dXNlcm5hbWU6cGFzc3dvcmQ= </br>

... should decode to: </br>

> username:password </br>

#### GET admin/accounts/{group-id}
After obtaining your credentials and logging in, you will find a variety of APIs available to monitor and explore your Service. For the sake of this discussion, we will primarily focus on the *Accounts* family of APIs, which all require authorization in form of 'basic <adm_tkn>'. To view the current credentials for a Service, you can use the GET to /admin/accounts/{group-id}. By default, your 'group-id' will be the zone-id of the Service, which is conveniently located in the Service URI.
#### POST admin/accounts/{group-id}/add
The most empowering API in the *Accounts* family is the POST to admin/accounts/{group-id}/add. By providing the required authorization ('basic <adm_tkn>') and the 'group-id' (Service zone-id by default), you can use this API to generate additional IDs which can then be used to configure additional EC Agent scripts.
#### Reusability of IDs
The IDs are capable of being reused, with some exceptions and limitations.
- When running multiple EC Servers simultaneously, there needs to be a 1:1 relationship between an ID used for the Server's *-aid* flag and the IP found in the Server's *-rht*
    - Running two identical Server scripts "locally" (or on a VM, etc) will mimick *scaling* as previously mentioned, and this is OK
- A single ID can be used for the *-aid* flag on multiple Clients simultaneously, provided each Client is assigned a different port (*-lpt*) to listen on, and the Client's *-tid* configuration is accurate
    - The Client uses the *-tid* flag to determine which Server, and ultimately which remote datasource, to access

#### ID Usage Example Diagrams
> Figure 1-a: EC Clients on prem reusing the same ID, this will be valid and functional
![valid configuration with multiple IDs](docs/properConfigLocalClients.png)
    
## FAQs
#### Q: Does each Gateway require an EC subscription?
No. The EC Service facilitates the generation and usage of EC Agent apps. The apps use a binary file whose behavior and function is controlled by a corresponding script. The EC Service "doesn't care" how many EC Agent apps you configure and run, with some caveats. The Gateway, Server, and Client are all Agents apps running the same binary files, distinguished by the flags used in the scripts, specifically the *-mod* flag.
#### Q: How much data and traffic can my EC Instance manage?
The EC Service instance is not concerned with the amount of data transferred. While we do recommend a separate EC instance for your 'prod' and 'non-prod' environments for the sake of isolation, there are tools and features that let one Service manage virtually "any" amount of traffic.
- You can scale your agents on Predix (including Gateways) with *cf scale app_name -i number_of_instances_desired*
    - Each Gateway instance can handle up to 50 concurrent sessions (Client-Server interactions)
- You can use the APIs in your Service URI to generate additional IDs beyond the two produced by default
    - You will need one ID per datasource IP, *-rht* flag on the Server. (1:1 ID:Servers)
    - You can use the same ID for all Client *-aid* flags, as long as you use different *-lpt* values and the *-tid* is configured for the correct Server (data source IP)
## Common Problems and Resolutions

## References and Further Resources
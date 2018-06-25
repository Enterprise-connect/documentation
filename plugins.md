<A NAME="top">
    
# Enterpise Connect Plugins
* [TLS](#tls)
* [VLAN (Linux only!)](#vlan)

The plugins available for EC are meant to expand the core functionality of EC based on user feedback and use cases which require some special behaviors and considerations. 

## TLS

The **TLS** plugin allows the EC Server to sit in the middle of communication between two points, and circumvents the breakdown of SSL communication by re-writing the requests.

### Usage

Some slight changes need to be made to the EC Server script to enable the plugin and signal its usage. Nothing needs to be done to the 'manifest.yml' for the EC Server agent itself (if pushing to Predix). Just like the agents themselves, there are binary executables associated with Linux, Windows and Mac.

### Examples

A modified Server script to run on Predix (notice the *-plg* flag!):

```bash
#!/bin/bash
chmod 755 ./tls_linux_sys;
./ecagent_linux_sys -mod server \
-grp group-name (probably same value as -zon) \
-aid q1w2e3 \
-cid myuaaclient -csc clientsecret -dur 1200 \
-oa2 https://normally-a-valid-uuid-here.predix-uaa.run.aws-usw02-pr.ice.predix.io/oauth/token \
-hst wss://some-example-gateway.run.aws-usw02-pr.ice.predix.io/agent \
-zon your-ec-service-zone-id -sst https://your-ec-service-zone-id.run.aws-usw02-pr.ice.predix.io \
-rht localhost -rpt 7979 \
-dbg -hca ${PORT} -plg
```

For a Server running on Predix (or Linux machine), you might have a plugins.yml file that looks like this:

```yaml
ec-plugin:
 tls:
 - status: active
   schema: https
   hostname: www.some.host.com
   tlsport: "443"
   proxy: ""
   port: "7979"
   command: ./tls_linux_sys
```

After both are configured properly, you may have a directory structure that looks something like this:

```
/ec_server
    │   ec.sh
    │   manifest.yml        
    │   ecagent_linux_sys
    │   plugins.yml
    │   tls_linux_sys           
```

Notice the agent binary and the tls binary are both Linux. Because they will be running in the same environment, they need to correspond in that regard.


<A HREF="#top">Back To Top</A>
## VLAN (only available in Linux environments)
The **VLAN** plugin allows the EC Client to create a Virtual LAN which mirrors the resources on the EC Server side. Rather than having to configure EC Clients for each target data source, one EC Client can be configured along with the plugins.yml to access as many data source IPs as necessary. Whereas normally you would access 'localhost' and some `-lpt` of your choosing, you will now have tools like pgAdmin and psql target the IPs you list in the plugins.yml. The EC Client will take over all connections to those IPs and then handle the rest. 


### Features  
* Enable/Disable VLAN forwarding based on VLAN flag
* VLAN header checking for resource forwarding
* VLAN ip/port mapping


### Example Agent Scripts

```bash
  ./ecagent_linux_sys -mod server -aid <VCAP_provided> \
  -grp <agent group, default group is the same as Predix-Zone-ID> \
  -cid <UAA_client_ID> -csc <UAA_client_Secret> \
  -dur 1200 -oa2 https://<predixUAA_URL>/oauth/token \
  -hst wss://<Predix_Gateway_App_URL>/agent \
  -zon <Predix-Zone-ID> -sst <EC-Service-URI> \
  -rht <resource IP> -rpt <resource port> \
  -dbg -hca ${PORT} -vln

  ./ecagent_linux_sys -mod client -aid <VCAP_provided> -tid <VCAP_provided> \
  -grp <agent group, default group is the same as Predix-Zone-ID> \
  -cid <UAA_client_ID> -csc <UAA_client_Secret> \
  -dur 1200 -oa2 https://<predixUAA_URL>/oauth/token \
  -hst wss://<Predix_Gateway_App_URL>/agent \ 
  -lpt <local listening port of your choosing, irrelevant for this use case> \
  -rpt <comma separated resource ports> \
  -dbg -pxy <your proxy info> \
  -plg vlan -vln
```
 

```yml
ec-plugin:
vlan:
- status: active
   ips: 10.93.210.30/32,10.93.210.32/32
   command: ./vlan
```   

The `status` must be set to 'active'. The `ips` should be the IPs of your data sources, you may need to be creative in finding these values. `Command` is how the plugin binary is started, should not be changed/configured.

<A HREF="#top">Back To Top</A>

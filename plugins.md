<A NAME="top">
    
# Enterpise Connect Plugins
* [TLS](#tls)
* [VLAN](#vlan)

The plugins available for EC are meant to expand the core functionality of EC based on user feedback and use cases which require some special behaviors and considerations. 

## TLS

The **TLS** plugin allows the EC Server to sit in the middle of communication between two points, and circumvents the breakdown of SSL communication by re-writing the requests.

### Usage

Some slight changes need to be made to the EC Server script to enable the plugin and signal its usage. Nothing needs to be done to the 'manifest.yml' for the Server agent itsef (if pushing to Predix). Just like the agents themselves, there are binary executables associated with Linux, Windows and Mac.

### Examples

A modified Server script to run on Predix (notice the *-plg* flag!):

```bash
#!/bin/bash
./ecagent_linux_sys -mod server -aid q1w2e3 \
-cid myuaaclient -csc clientsecret -dur 1200 \
-oa2 https://normally-a-valid-uuid-here.predix-uaa.run.aws-usw02-pr.ice.predix.io/oauth/token \
-hst wss://some-example-gateway.run.aws-usw02-pr.ice.predix.io/agent \
-zon your-ec-service-zone-id -sst https://your-ec-service-zone-id.run.aws-usw02-pr.ice.predix.io \
-rht some.example.resource -rpt 7979 \
-dbg -hca ${PORT} -plg tls
```

For a Server running on Predix (or Linux machine), you might have a plugins.yml file that looks like this:

```yaml
ec-plugin:
 tls:
 - status: Active
   schema: https
   hostname: some.example.data.source.predix.io
   tlsport: "443"
   proxy: ""
   port: "7979"
   command: ./tls_linux -v
 vlan:
 - status: Inactive
 - ips: 
```

After both are configured properly, you may have a directory structure that looks something like this:

```
/ec_server
    │   ec.sh
    │   manifest.yml        
    │   ecagent_linux_sys
    │   plugins.yml
    │   tls_linux           
```

Notice the agent binary and the tls binary are both Linux, because they will be running in the same environment, they need to correspond in that regard.


<A HREF="#top">Back To Top</A>
## VLAN (LINUX ONLY!)
The **VLAN** plugin allows the EC Client to be able to speak to a variety of IPs and ports on the backend. Rather than having to configure EC Clients for each target data source, one EC Client can be configured along with the plugins.yml to access as many data source IPs as necessary.


### Features  
* Enable/Disable VLAN forwarding based on VLAN flag
* VLAN header checking for resource forwarding
* VLAN ip/port mapping


### Examples
<pre><code>  
ecagent_linux_sys -aid r4t5y6 -tid q1w2e3 \ 
-cid myuaaclient -csc clientsecret -dur 1200 \ 
-oa2 https://normally-a-valid-uuid-here.predix-uaa.run.aws-usw02-pr.ice.predix.io/oauth/token \
-hst wss://some-example-gateway.run.aws-usw02-pr.ice.predix.io/agent \ 
-dbg -lpt 3984 <b>-rpt 1525,1554 -vln</b>
</code></pre>
 
Notice the bold sections. The rpt flag should have local listener and remote listener(scan) ports. In this example case it was 1525 and 1554 and so we've added those 2 ports. We have also included the `-vln` flag required to enable the plugin's usage.

```yml
ec-plugin:
vlan:
- status: active
   ips: 10.93.210.30/32,10.93.210.32/32,10.93.210.31/32,10.220.96.13/32,192.168.11.13/32,10.93.210.23/32 (These are the ips of the SCAN listener and local listener)
   port: 8995
   command: ./vlan
```   

<A HREF="#top">Back To Top</A>
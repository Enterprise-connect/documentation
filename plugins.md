<A NAME="top">
    
# Enterpise Connect Plugins
* [TLS](#tls)
* [VLAN](#vlan)

To take advantage of either plugin's functionality, a user must provide a 'plugins.yml' in their agent directory, as well as the relevant plugin binary, explained below.

## TLS

### Usage

Some slight changes need to be made to the EC Server script to enable the plugin and signal its usage. Nothing needs to be done to the 'manifest.yml' for the Server agent itsef (if pushing to Predix). Just like the agents themselves, there are binary executables associated with Linux, Windows and Mac.

### Examples

A modified Server script to run on Predix (notice the *-plg* flag!):

```bash
#!/bin/bash
./ecagent_linux_sys -mod server -aid q1w2e3 -cid myuaaclient -csc clientsecret -dur 1200 -hst wss://some-example-gateway.run.aws-usw02-pr.ice.predix.io/agent -oa2 https://normally-a-valid-uuid-here.predix-uaa.run.aws-usw02-pr.ice.predix.io/oauth/token -zon your-ec-service-zone-id -sst https://your-ec-service-zone-id.run.aws-usw02-pr.ice.predix.io -rht some.example.data.source.predix.io -rpt 7979 -dbg -hca ${PORT} -plg tls
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


<A HREF="#top">Back To Top</A>
## VLAN
### Features
* Enable/Disable VLAN forwarding based on VLAN flag.
* VLAN header checking for resource forwarding.
* VLAN ip/port mapping.

<A HREF="#top">Back To Top</A>
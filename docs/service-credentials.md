# Service Credentials
This page aims to help users understand Enterprise Connect ('EC') credentials, how to access them, and where to use them.

* [Accessing Credentials](#accessing-credentials)
* [How-To](#how-to)

---
---

## Accessing Credentials
There are a couple of methods readily available to users in order to obtain credentials for an EC Service. We strongly recommend the use of [service keys](#service-key) for all users, especially those new to Cloud Foundry, as this adds the least cognitive overhead to an already esoteric process.

* [Service Key](#service-key)
* [Service Binding](#service-binding)

### Service Key
Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to create a Service Key:

```
cf create-service-key SERVICE_INSTANCE SERVICE_KEY
```

After creating a Service Key, use this command to view the EC credentials:

```
cf service-key SERVICE_INSTANCE SERVICE_KEY
```

This should return a JSON similar to the one below:

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

### Service Binding
Using the [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html), use the following command to bind the service to an application in the same org/space:

```
cf bind-service APP_NAME SERVICE_INSTANCE
```

After binding the service to the application, use this command to view the EC credentials:

```
cf env APP_NAME
```

This should return a JSON similar to the one below:

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

## Pro Tips
Here are the most common [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html) commands related to service keys and service binding.

### Service Key Commands

#### List
```
NAME:
   service-keys - List keys for a service instance

USAGE:
   cf service-keys SERVICE_INSTANCE

EXAMPLES:
   cf service-keys mydb

ALIAS:
   sk
```

#### Read
```
NAME:
   service-key - Show service key info

USAGE:
   cf service-key SERVICE_INSTANCE SERVICE_KEY

EXAMPLES:
   cf service-key mydb mykey

OPTIONS:
   --guid      Retrieve and display the given service-key\'s guid.  All other output for the service is suppressed.
```

#### Create
```
NAME:
   create-service-key - Create key for a service instance

USAGE:
   cf create-service-key SERVICE_INSTANCE SERVICE_KEY [-c PARAMETERS_AS_JSON]

   Optionally provide service-specific configuration parameters in a valid JSON object in-line.
   cf create-service-key SERVICE_INSTANCE SERVICE_KEY -c '{"name":"value","name":"value"}'

   Optionally provide a file containing service-specific configuration parameters in a valid JSON object. The path to the parameters file can be an absolute or relative path to a file.
   cf create-service-key SERVICE_INSTANCE SERVICE_KEY -c PATH_TO_FILE

   Example of valid JSON object:
   {
      "permissions": "read-only"
   }

EXAMPLES:
   cf create-service-key mydb mykey -c '{"permissions":"read-only"}'
   cf create-service-key mydb mykey -c ~/workspace/tmp/instance_config.json

ALIAS:
   csk

OPTIONS:
   -c      Valid JSON object containing service-specific configuration parameters, provided either in-line or in a file. For a list of supported configuration parameters, see documentation for the particular service offering.
```

#### Delete
```
NAME:
   delete-service-key - Delete a service key

USAGE:
   cf delete-service-key SERVICE_INSTANCE SERVICE_KEY [-f]

EXAMPLES:
   cf delete-service-key mydb mykey

ALIAS:
   dsk

OPTIONS:
   -f      Force deletion without confirmation
```

### Service Binding Commands

#### Bind
```
NAME:
   bind-service - Bind a service instance to an app

USAGE:
   cf bind-service APP_NAME SERVICE_INSTANCE [-c PARAMETERS_AS_JSON] [--binding-name BINDING_NAME]

   Optionally provide service-specific configuration parameters in a valid JSON object in-line:

   cf bind-service APP_NAME SERVICE_INSTANCE -c '{"name":"value","name":"value"}'

   Optionally provide a file containing service-specific configuration parameters in a valid JSON object.
   The path to the parameters file can be an absolute or relative path to a file.
   cf bind-service APP_NAME SERVICE_INSTANCE -c PATH_TO_FILE

   Example of valid JSON object:
   {
      "permissions": "read-only"
   }

   Optionally provide a binding name for the association between an app and a service instance:

   cf bind-service APP_NAME SERVICE_INSTANCE --binding-name BINDING_NAME

EXAMPLES:
   Linux/Mac:
      cf bind-service myapp mydb -c '{"permissions":"read-only"}'

   Windows Command Line:
      cf bind-service myapp mydb -c "{\"permissions\":\"read-only\"}"

   Windows PowerShell:
      cf bind-service myapp mydb -c '{\"permissions\":\"read-only\"}'

   cf bind-service myapp mydb -c ~/workspace/tmp/instance_config.json --binding-name BINDING_NAME

ALIAS:
   bs

OPTIONS:
   --binding-name      Name to expose service instance to app process with (Default: service instance name)
   -c                  Valid JSON object containing service-specific configuration parameters, provided either in-line or in a file. For a list of supported configuration parameters, see documentation for the particular service offering.
```

#### Retrieve ENVs
```
NAME:
   env - Show all env variables for an app

USAGE:
   cf env APP_NAME

ALIAS:
   e
```

#### Unbind
```
NAME:
   unbind-service - Unbind a service instance from an app

USAGE:
   cf unbind-service APP_NAME SERVICE_INSTANCE

ALIAS:
   us
```

[back to top](#service-credentials)
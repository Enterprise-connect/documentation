# Troubleshooting

(WIP)

---
---

## Problem: My database connection is failing or intermittent, I believe EC is failing
Typically, when there is a problem with EC, you will have no connectivity whatsoever. If the connectivity is strong for 'a while', but then begins to deteriorate or fail, please take a look at your Gateway /health. If you see active 'sessions' when there should be no activity (ie, `client pool == 0 || client pool < session count`), this is why your connectivity begins to fail, and it's because the application is not sending/signaling an `EOF`. Example:

```psql
psql << EOF
\copy (select * from dummy_data limit 100) to 'dummy_data.csv' csv;
\q
EOF
```

## Problem: '[EC Client] error while adding the client inst.'
This error occurs when the EC Client script is configured to connect to an invalid Gateway URL via the *-hst* flag, or when it tries to connect to an EC Server through an EC Gateway with no active super connections matching that Server's `-aid` value.

## Problem: General connectivity (SuperConnection, etc) can be established but deteriorates immediately on end-to-end usage
While there are a variety of potential causes for this symptom, the most likely are:

- The EC agents are not running the same version of the binary, or outdated versions of the binary
- The Service requires an update to be compatible with current/recommended agents
- You have attempted to scale an EC Gateway in Predix Select, which is currently not supported due to the platform

## Problem: The Service is repeatedly crashing or failing in very consistent intervals

>(much more frequently than the known issue where an idle EC Service will crash roughly every 2-3 days)

This is likely an issue with the relationship between your UAA Client and how often the Server and Client are fetching/refreshing tokens. While this is a fairly common source of support tickets, this is easily solved on the user's end by examining the *-dur* flag on your Server and Client. Please be sure the value used for this flag is less than the *Token Validity* values of your UAA Client.

**The easiest way to verify this:**

Start up an EC Server or EC Client. After it starts up and reports the version, it will fetch a bearer token (which will be visible if the *-dbg* flag is enabled), and then it will print something out along these lines:

> [EC Client] 2018/9/31 25:61:00 Token refreshed. The token will be expired in ***x*** minutes. Approx. ***y*** minutes to the next auto-refresh

if (y >= x) { You are going to have issues };

## Problem: EC Server agent is getting 404 trying to reach the EC Gateway
The solutions to this problem range from "simple fix" to a Predix Support ticket. The easiest and most likely causes are:
- Have you verified the EC Gateway is up and running?
- Does the *-hst* flag properly reflect the EC Gateway URL in the correct format?
    - *-hst wss://gateway-url/agent*
    
Beyond these simple fixes, if the 404 error is including the name of your current Gateway app/url, and you have pushed or updated this Gateway in the past, this could be due to the existence of "phantom" apps which were not properly deleted in Cloud Foundry. In such cases, only the Predix Support team has the tools and access to identify and correct such anomalies. In this event, they will need the 'gtwId's of the offending apps, which you can get from the Gateway list at the Service URI, or in the EC Server logs near the 404 message. They can use these Ids to find and properly destroy the bad Gateway apps.

[back to top](#troubleshooting)
[Documentation Home](https://enterprise-connect.github.io/documentation/)
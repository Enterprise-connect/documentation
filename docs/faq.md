# FAQ

(WIP)

If you have a question not seen here, please [ask in our Issues](https://github.com/Enterprise-connect/documentation/issues)! Even if it is not 'frequently asked', any good question that the EC team believes will help other users will be added to this or related pages!

---
---

### Q: How much data and traffic can my EC Instance manage?
The EC Service instance is not concerned with the amount of data transferred. While we do recommend a separate EC instance for your 'prod' and 'non-prod' environments for the sake of isolation, there are tools and features that let one Service manage virtually "any" amount of traffic.

- You can scale your agents on Predix (including Gateways) with *cf scale app_name -i number_of_instances_desired*
    - Each Gateway instance can handle up to 50 concurrent sessions (Client-Server active, in-flight connections)
- You can use the APIs in your Service URI to generate additional IDs beyond the two produced by default
    - You will need one ID per datasource IP, *-rht* flag on the Server. (1:1 ID:Servers)
    - You can use the same ID for all Client *-aid* flags, as long as you use different *-lpt* values and the *-tid* is configured for the correct Server (data source IP)

> While the root cause is currently unknown, bad behavior has been observed in EC Services where the [accounts APIs](./service.md#apis) have been used to relative excess. While we do not have any specific limits set, if you are experience issues after creating many [groups](./service.md#groups) or [IDs](./service.md#ids), consider breaking these up over more EC Service subscriptions, or reach out to the EC team to evaluate the need/usage.
    
### Q: Are there any data bandwidth restrictions over EC?
No, Enterprise Connect does not set any limits on bandwidth usage.

### Q: How much does EC cost?
Please see our [Predix page](https://www.predix.io/services/service.html?id=2184) for information on pricing.

### Q: Can I get EC without going through Predix?
We hope to be available in other markets by the end of 2019.

[back to top](#faq)
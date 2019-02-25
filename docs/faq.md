# FAQ
If you have a question not seen here, please [ask in our Issues](https://github.com/Enterprise-connect/documentation/issues)! Even if it is not 'frequently asked', any good question that the EC team believes will help other users will be added to this or related pages!

* [Technical](#technical)
* [Pricing](#pricing)
* [Predix](#predix)
* [Language-Specific Use-Cases](#language-specific-use-cases)

---
---

## Technical

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

## Pricing

### How much does EC cost?
Please see our [Predix page](https://www.predix.io/services/service.html?id=2184) for information on pricing.

### Q: Does each Gateway require an EC subscription?
Pending an update, only one Gateway can be deployed at this time, but it can be scaled to multiple instances, allowing for the management of increased traffic volumes.

## Predix

### Q: Can I get EC without going through Predix?
We hope to be available in other markets by the end of 2019.

## Language-Specific Use-Cases

### Q: Can you help me leverage EC in between a specific language based on libraries and dependencies my team uses?
Maybe? The EC Agent's behavior does not change between Linux and Windows, and does not know if Node or Java executed it. Because of this, it is largely the user's responsibility to understand how the EC Agent behaves and write their applications accordingly. In the event we have the resources to advise on specific use-cases, we tend to stop at 'proving' connectivity in the most simplistic, 'happy path' form.

### Q: Do you have any examples?
Yes and no. We have some abstracted examples floating around, but they will not, by any stretch of the imagination, provide adequate substitution for users understanding EC on the process level.

[back to top](#faq)
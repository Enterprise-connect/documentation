# Support

To expedite the resolution of your issue, we ask that you please take a moment to [file a ticket with Predix support](https://www.predix.io/support/), as we do not have a FTE for support. The less time engineers spend on support, the faster we can fix bugs and bring new features that users request! In the ticket, please include the following items to ensure the fastest resolution possible:

- Server logs including Agent version (Agent versions are always shown at start-up, may require restart of Agent to view)
- The Server script used
- Gateway logs including Agent version
- The Gateway script used
- Client logs including Agent version
- The Client script used
- JSON from the Service URL /v1/info/ (be sure to include the trailing / in the URL)
- EC Gateway application URL

Please be aware that tickets with incomplete information may take **considerably** longer to resolve due to the need for follow-up emails (which will most likely be requesting the missing information). The EC Team and the Predix support staff have found that the vast majority of users’ issues are related to problems that can be identified through the items mentioned above.

## Helpful Items
There are some tools readily available to users to monitor various aspects of the EC Service.

### Gateway Health Check
> https://your-gateway-app-url/health

This is going to show you the version of the Gateway, the current Sessions (Servers/Clients currently transmitting data), the active SuperConnections (EC Servers correctly connected to the Gateway), ClientPool, and various memory data. This can be curled and returns a JSON which can be parsed for advanced, automated/program-based monitoring.å

### EC Service URI
> You can find this in your [Service Key](./service-credentials.md#service-key)

In your browser, you can navigate here and click on API Docs to gain access to a wide variety of APIs you can use to diagnose and monitor your Service. Our [Service page](./service.md) has all the information you need to understand and [use the APIs](./service.md#apis).

### Join the EC Usergroup Flowdock: Come join the discussion!
The [EC Usergroup Flowdock](https://www.flowdock.com/invitations/44765fcbae5a36d0eff83c9536f87223044ad748) is a great place to discuss with other users and the EC team your successes, as well as your struggles. The EC team do their best to monitor and answer questions in “real time” here, and we use this and other feedback to prioritize our agile development. It’s a great place to network with other EC Users. Perhaps you are developing a custom solution which leverages EC – this is a great place to share your progress and to also find peer assistance with bridging gaps. ***More dialogue = more ideas = more solutions***.
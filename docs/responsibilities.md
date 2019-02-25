# Responsibilities
This page is aimed in helping users understand their responsibilities as users, and our responsibilities to them as a conscientious product team. 

* [We Are Here For You](#here-for-you)
* [Dude, Where's My Gateway?](#dude-wheres-my-gateway)
* [Logs](#logs)
* [Pro Tips](#pro-tips)

---
---

## Here For You
Are you an external customer? *We are here for you.*

Are you an internal customer? *We are here for you.*

Are you part of Predix Support? *We are here for you.*

Are you a member of an Operations team? *We are here for you.*

No matter what road brought you to Enterprise Connect, ***we are here for you.***

We will always do our best to ensure our users (and our users' users!) are successful in their projects that leverage Enterprise Connect. Unfortunately, there are logical and logistical limitations to what support we can provide. Rest assured, we will never shy away from helping anyone on the basis of inconvenience. 

## Dude, Where's My Gateway?
One of the more common, and unfortunate, situations we face with helping users stems from a fundamental misunderstanding of what kind of user data we 'collect'. Outside of the mechanisms in place by Predix, Nurego, etc., we do not actually persist any user data. 

The situation seems to play out like this:

- a team needs EC
- the team tasks someone with learning and implementing EC
- this is accomplished
- the individual tasked with setting up EC did it all from their local machine and has since left the team
- requirements change, or EC changes, and the working configuration stops working

This is the point in which we start receiving emails from users asking for assistance in untangling everything. The content can vary, but the message between the lines is always the same,

> "We thought the EC team had some kind of dashboard that shows every customer everywhere, and can find which orgs and spaces EC components were deployed to"

This is not the case. We have access to all EC services running, but we cannot tell whose is whose. To try to put this into perspective, it is sometimes easier to just 'forget the whole thing' and start over from scratch than it is to hunt down all the various support people we would need to hunt down IPs, etc. 

**There is no database. You must document everything.**

## Logs
Logs are the king of the debug hill. No amount of verbal/text anecdotes can replace a log. No amount of server logs can replace a client log. No amount of client logs can replace a gateway log. See where we are going?

For support issues that are not typo-related, the EC team needs logs. The more relevant you can provide, the faster a resolution can be found.

### Pro Tips
- Use tools like GitHub to track and document everything related to your EC configuration
- Join the EC Usergroup on Flowdock

[back to top](#responsibilities)
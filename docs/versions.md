# Versions

(WIP)

This page is aimed to explain why the versions matter for the EC Agent as well as the EC Service itself.

* [Users](#users)
* [Developers](#developers)
* [Version Table](#version-table)
* [Pro Tips](#pro-tips)

---
---

## For Users
EC is available in multiple cloud environments. Due to differences between ones such as Select/East and Basic/West, more than one versioning/tagging of the EC Service was maintained for a long time. To find your EC Service's version, go to the /info/ (be sure to include the trailing '/') endpoint of your [EC Service URI](./service-credentials.md#understanding-credentials). It will return a JSON that shows the version.

## For Developers
Because the dev environment for EC uses a different API version (v1beta instead of v1) than production, this distinction is addressed by using the dev/beta binary agent, which has a separate version/tag than the production agent. When working with a development instance of the EC Service, the production binary will attempt to hit endpoints that do not exist, and the same holds true when testing a production instance of the EC Service with the development/beta binary.

## Version Table

|           Environment           | Latest Service Version | Compatible Agent Version(s) |
|:-------------------------------:|:----------------------:|:---------------------------:|
|  run.aws-usw02-pr.ice.predix.io |      v1.kyushu.146     |       v1.hokkaido.204       |
| run.aws-usw02-dev.ice.predix.io |   v1beta.sendai.1080   |     v1beta.fukuoka.1668     |

## Pro Tips
- Bookmark this page

[back to top](#versions)
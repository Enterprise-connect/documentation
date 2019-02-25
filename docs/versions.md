# Versions
This page is aimed to explain why the versions matter for the EC Agent as well as the EC Service itself.

* [Users](#users)
* [Developers](#developers)

---
---

## For Users
EC is available in multiple cloud environments. Due to differences between ones such as Select/East and Basic/West, more than one versioning/tagging of the EC Service was maintained for a long time. To find your EC Service's version, go to the /info/ (be sure to include the trailing '/') endpoint of your [EC Service URI](./service-credentials.md#understanding-credentials). It will return a JSON that shows the version.

## For Developers
Because the dev environment for EC uses a different API version (v1beta instead of v1) than production, this distinction is addressed by using the dev/beta binary agent, which has a separate version/tag than the production agent. When working with a development instance of the EC Service, the production binary will attempt to hit endpoints that do not exist, and the same holds true when testing a production instance of the EC Service with the development/beta binary.

[back to top](#versions)
# Versions

* [Developers](#developers)
* [Pro Tips](#pro-tips)

|           Environment           | Latest Service Version | Compatible Agent Version |
|:-------------------------------:|:----------------------:|:------------------------:|
|  run.aws-usw02-pr.ice.predix.io |      v1.kyushu.146     |      [v1.hokkaido.204](https://github.com/Enterprise-connect/ec-x-sdk/tree/v1/dist)     |
| run.aws-usw02-dev.ice.predix.io |   v1beta.sendai.1080   |    [v1beta.fukuoka.1668](https://github.com/Enterprise-connect/ec-x-sdk/tree/v1beta/dist)   |

To find your EC Service's version, go to the /info/ (be sure to include the trailing '/') endpoint of your [EC Service URI](./service-credentials.md#understanding-credentials). It will return a JSON that shows the version.

```bash
# Example Service URI
# https://xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.run.something.predix.io

curl https://xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.run.something.predix.io/v1/info/

# returns something like:
# {
# 	"sid": "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
# 	"ver": "v1",
# 	"build": "v1.kyushu.146", <--- That is your Service Version
# 	"plan": "Oneway-TLS"
# }
```

---
---

## Developers
Because the dev environment for EC uses a different API version (v1beta instead of v1) than production, this distinction is addressed by using the dev/beta binary agent, which has a separate version/tag than the production agent. When working with a v1beta.sendai instance of the EC Service, the v1.hokkaido binary will attempt to hit endpoints that do not exist, and the same holds true when testing a v1.kyushu instance of the EC Service with the v1beta.fukuoka.1668 binary.

## Pro Tips
- Bookmark this page

[back to top](#versions)

[documentation home](https://enterprise-connect.github.io/documentation/) 
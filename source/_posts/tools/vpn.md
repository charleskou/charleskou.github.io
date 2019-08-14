---
title: vpn
tags: [Tool]
date: 2019-08-13 14:47:04
---

## 注册
连接vpn后在管理员模式命令行执行以下命令进行注册
```
REG ADD HKLM\SYSTEM\CurrentControlSet\Services\PolicyAgent /v AssumeUDPEncapsulationContextOnSendRule /t REG_DWORD /d 0x2 /f
REG ADD HKLM\SYSTEM\CurrentControlSet\Services\RasMan\Parameters /v ProhibitIpSec /t REG_DWORD /d 0x0 /f
REG ADD HKLM\SYSTEM\CurrentControlSet\Services\PolicyAgent /v AllowL2TPWeakCrypto /t REG_DWORD /d 0x1 /f
```


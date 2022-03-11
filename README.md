# MSI-MAG-B560M-i5-11500

|            |                                       |
| --------   |---------------------------------------|
| Mobo       | MSI-MAG-B560-MORTAR-WIFI              |
| CPU        | Intel(R) Core(TM) i5-11500            |
| RAM        | Kingston DDR4 3200  32GBx2            |
| GPU        | ONDA Radeon RX 560 典范 4GD5            |
| Network    | Realtek PCIe 2.5Gb Ethernet (RTL8125) |
| WIFI       | BCM94360CD                            |
| Disk       | WD SN850 PCIE 4.0                     |
| Input      | Apple Magic Trackpad 2                |
| Bootloader | OpenCore 0.7.9 Release                |
| System     | macOS 12.2.1                          |



## 状态
 - USB定制了前后面板USB、Type c、主板usb。
 - AppleALC 897参考 [R-a-s-c-a-l的配置](https://github.com/R-a-s-c-a-l/MSI-MAG-B560M-i7-11700/issues/1),无修改，使用了原版
 - Wi-Fi 蓝牙 (没放驱动)，使用了小齐家的四天线BCM94360CD。最开始用FV T919发现无线信号差，速率只有200Mbps，退了。
 - AirDrop、AirPods自动切换、iWatch解锁、屏幕镜像到AirPlay设备、Airplay投射到本设备功能正常。这些功能依赖于苹果原装网卡和蓝牙。
 - 使用Apple Magic Trackpad 2作为输入可以带来刚好的操作体验
 - 睡眠唤醒功能正常
 - SMBIOS MacPro 7,1

## changelog
### 2022.03.11
- 解决了睡眠问题，可以正常睡眠唤醒，在bios中禁用了核显。暂时没搞定ssdt方式禁用核显
- 增加 SSDT-USBW.aml 和 USBWakeFixup.kext 解决usb唤醒。

## 说明

### USB定制

- 更新使用SSDT方式定制USB在xhci中增加MacOS系统使用节点,无需屏蔽acpi表。同时也不影响其他系统对USB的识别。

  发现HS02接口是该主板的内置蓝牙HCI，增加了屏蔽。避免干扰免驱卡蓝牙。

<img src="img/image-20220304155726895.png" alt="image-20220304155726895" style="zoom:50%;" />

当前USB定制展示

<img src="img/image-20220304161421059.png" alt="image-20220304161421059" style="zoom:50%;" />

| 名称 | 类型          | 说明                                 |
| ---- | ------------- | ------------------------------------ |
| HS01 | Typec 2.0     | SS01 主板后面板 Typec接口的2.0模式   |
| HS03 | USB 2.0       | 主板后面板三个蓝色USB3接口的2.0模式  |
| HS05 | USB 2.0       | SS04 前面板usb3接口的2.0模式         |
| HS06 | USB 2.0       | SS05 前面板usb3接口的2.0模式         |
| HS07 | USB 2.0       | 后面板四个usb2.0接口左1              |
| HS08 | USB 2.0       | 后面板四个usb2.0接口左2              |
| HS09 | USB 2.0       | 后面板四个usb2.0接口左3              |
| HS10 | USB 2.0       | 后面板四个usb2.0接口左4              |
| HS11 | 内建          | 主板上USB插口，接了pci网卡上的蓝牙   |
| HS14 | 内建          | 主板RGB控制器                        |
| SS01 | Typec 3.0 10G | 主板后面板Typec接口的3.0模式         |
| SS02 | USB 3.0 5G    | 主板后面板三个蓝色USB3接口           |
| SS03 | Typec 3.0 10G | 猜测是主板内置usb 3.0 1G接口，未测试 |
| SS04 | Usb 3.0 5G    | 前面板usb 3.0接口                    |
| SS05 | Usb 3.0 5G    | 前面板usb 3.0接口                    |



- 使用USBMap定制USB可以参考 release v1.0版本中的 USBMap.kext

<img src="img/iShot2022-02-25%2013.06.48.png" alt="iShot2022-02-25 13.06.48" style="zoom:50%;" />



### AppleALC 897
AppleALC组件使用了原版方案，使用`device-id=D07A0000`、`layout-id=66`等参数。

<img src="img/iShot2022-02-25%2016.06.17.png" alt="iShot2022-02-25 16.06.17" style="zoom:50%;" />

----------------
### 展示


```
EFI
├── BOOT
│   └── BOOTx64.efi
└── OC
    ├── ACPI
    │   ├── SSDT-AWAC-DISABLE.aml
    │   ├── SSDT-EC-USBX.aml
    │   ├── SSDT-PLUG.aml
    │   ├── SSDT-SBUS-MCHC.aml
    │   ├── SSDT-USBW.aml
    │   └── SSDT-USB-Ports-B560M.aml
    ├── Drivers
    │   ├── ExFatDxe.efi
    │   ├── HfsPlus.efi
    │   ├── OpenCanopy.efi
    │   └── OpenRuntime.efi
    ├── Kexts
    │   ├── AppleALC.kext
    │   ├── Lilu.kext
    │   ├── LucyRTL8125Ethernet.kext
    │   ├── RestrictEvents.kext
    │   ├── SMCProcessor.kext
    │   ├── SMCSuperIO.kext
    │   ├── VirtualSMC.kext
    │   ├── WhateverGreen.kext
    │   └── USBWakeFixup.kext
    ├── OpenCore.efi
    └── config.plist

14 directories, 13 files
```

<img src="img/iShot2022-02-25%2013.06.24.png" alt="iShot2022-02-25 13.06.24" style="zoom:50%;" />

<img src="img/iShot2022-02-25%2013.06.35.png" alt="iShot2022-02-25 13.06.35" style="zoom:50%;" />

<img src="img/iShot2022-02-25%2013.07.09.png" alt="iShot2022-02-25 13.07.09" style="zoom:50%;" />

<img src="img/iShot2022-02-25%2013.12.52.png" alt="iShot2022-02-25 13.12.52" style="zoom:50%;" />

<img src="img/image-20220304161650335.png" alt="image-20220304161650335" style="zoom:50%;" />

----------------
## 鸣谢

\- [Mieze](https://github.com/Mieze) 提供 [LucyRTL8125Ethernet](https://github.com/Mieze/LucyRTL8125Ethernet).

\- [Acidanthera](https://github.com/acidanthera) 提供 [AppleALC](https://github.com/acidanthera/AppleALC), [RestrictEvents](https://github.com/acidanthera/RestrictEvents), [Lilu](https://github.com/acidanthera/Lilu), [OcBinaryData](https://github.com/acidanthera/OcBinaryData), [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg), [VirtualSMC](https://github.com/acidanthera/VirtualSMC) ,[WhateverGreen](https://github.com/acidanthera/WhateverGreen)

\- 参考了配置文件[R-a-s-c-a-l](https://github.com/R-a-s-c-a-l/MSI-MAG-B560M-i7-11700), [sqlsec](https://github.com/sqlsec/MSI-MAG-B560M-MORTAR-i7-10700)

\- 参考 [USB MITTELS SSDT DEKLARIEREN](https://www.hackintosh-forum.de/forum/thread/54986-usb-mittels-ssdt-deklarieren/?pageNo=1)
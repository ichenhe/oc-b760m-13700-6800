⚠️ **WARNING: You must add your own serial number in `EFI/OC/config.plist`.** Related fields are: `PlatformInfo - Generic - MLB / SystemProductName / SystemSerialNumber / SystemUUID`. You can generate some random numbers by [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS). See [official guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#platforminfo) for details.

## Hardware

- **Motherboard:** MSI B760M MORTAR MAX WIFI D5
  - Ethernet: Realtek 2.5Gbps
  - Audio Codec: Realtek ALC897 Codec
  - Wireless (Bluetooth): Intel Wi-Fi 6E
- **CPU:** Intel Core i7 13700 (iGPU not working)
- **dGPU:** AMD RX 6800
- **Storage:** WD_BLACK SN850X 2000GB
- **RAM:** Gloway DDR5 6400 16GB*2

## Software

- **OS:** macOS Ventura 13.6.5
- **Bootloader:** OpenCore 0.9.9
- ❌ Not tested on macOS 14 

## What's working

- [x] OpenCore GUI Picker
- [x] dGPU
- [x] Restart / Shutdown
- [x] Sleep / Wake
- [x] WiFi & Bluetooth
- [x] USB
- [x] macOS & Windows 11

## BIOS Settings

MSI B760M MORTAR MAX WIFI D5 ver E7E01IMS.H40 build 07/12/2023

SETTINGS - Advanced:
 - PCIe/PCI Sub-system Settings - Re-size BAR Support: Disabled
 - Integrated Peripherals - External SATA Controller Mode: AHCI Mode
 - Integrated Graphics Configuration
   - Initiate Graphic Adapter: PEG
   - IGD Multi-Monitor: Enabled
- USB Configuration - XHCI Hand-off: Enabled
- BIOS CSM/UEFI Mode: UEFI

SETTINGS - Boot:
- MSI Fast Boot: Disabled
- Fast Boot: Disabled

SETTINGS - Security - Secure Boot - Secure Boot: Disabled

OC - CPU Features:
- Intel Virtualization Tech (VT-x): Enabled
- Intel VT-D Tech: Disabled
- CFG Lock: Disabled


## Kexts

| Name                                                         | Version       |      |
| ------------------------------------------------------------ | ------------- | ---- |
| [Lilu](https://github.com/acidanthera/Lilu/releases)         | 1.6.7         |      |
| [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases)<br />SMCProcessor<br />SMCSuperIO | 1.3.2         |      |
| [SMCRadeonGPU<br />RadeonSensor](https://github.com/NootInc/RadeonSensor/releases) | 1.3.0         |      |
| [WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases) | 1.6.6         |      |
| [AppleALC](https://github.com/acidanthera/AppleALC/releases) | 1.8.9         |      |
| [LucyRTL8125Ethernet](https://www.insanelymac.com/forum/files/file/1004-lucyrtl8125ethernet/) | 1.1.0         |      |
| [USBToolBox](https://github.com/USBToolBox/kext)             | 1.1.1         |      |
| [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM/releases) | 2.6.8         |      |
| [AirportItlwm](https://github.com/OpenIntelWireless/itlwm/releases) | 2.2.0 Ventura |      |
| [IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)<br />IntelBTPatcher | 2.4.0         |      |
| [NVMeFix](https://github.com/acidanthera/NVMeFix/releases)   | 1.1.1         |      |
| [CPUTopologyRebuild](https://github.com/b00t0x/CpuTopologyRebuild) | 1.1.0         |      |
| [RestrictEvents](https://github.com/acidanthera/RestrictEvents) | 1.1.3         |      |

## Config

Most of efi configurations are based on [Dortania's guide for Comet Lake](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#acpi), but there are some differences for 13th CPU (thanks [大头蔡Cass](https://www.youtube.com/watch?v=qcOpeg9E1fQ)).

- CPU's SSDT is `SSDT-PLUG-ALT.aml` provided with opencore pkg.
- Kernel-Emulate: Emulate Comet Lake, details:
  - Cpuid1Data: `55060A00000000000000000000000000`
  - Cpuid1Mask: `FFFFFFFF000000000000000000000000`
- Kernel-Quirks: enable `provideCurrentCpulnfo`
- NVRAM-Add-4D1FDA02...: add new item `revcpu` with value 1 (number)
- NVRAM-Del-4D1FDA02...: add item `revcpu`
- PlatformInfo-Generic-ProcessorType: `3841` (need `RestrictEvents.kext`)

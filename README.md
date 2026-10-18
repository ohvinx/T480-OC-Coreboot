# OpenCore ThinkPad T480

OpenCore configuration for Lenovo ThinkPad T480.  
OpenCore version: **1.0.8**.

### Firmware

Coreboot  
MrChromebox EDK2 

`SecureBootModel` is disabled  
as it conflicts with EDK2 

`ProtectMemoryRegions` must be enabled in order to boot  

### Hardware

- [x] Intel UHD Graphics 620
- [x] NVIDIA MX150 disabled
- [x] NVMe support
- [x] USB mapping
- [x] Ethernet kext loaded
- [x] Wi-Fi/bluetooth kext loaded

### Software

- [x] QE/CI graphics stack
- [x] Metal graphics stack
- [x] Battery monitoring support
- [x] CPU temperature support
- [x] SMC sensor support
- [x] USB port mapping
- [x] Intel wireless support
- [x] Intel Bluetooth support
- [x] Audio codec support

## System

| **Component** | **Configuration** |
|---|---|
| **Laptop** | Lenovo ThinkPad T480 |
| **CPU** | Intel Core i5-8350U |
| **Generation** | Coffee Lake |
| **Memory** | 32 GB |
| **iGPU** | Intel UHD Graphics 620 |
| **dGPU** | NVIDIA MX150, disabled |
| **Firmware** | Coreboot |
| **payload** | MrChromebox EDK2 fork |
| **OpenCore** | 1.0.8 |
| **macOS** | macOS Tahoe |

## EFI structure

```text
config.plist
config-backup.plist
ACPI/
  SSDT-HPET.aml
  SSDT-PLUG-ALT.aml
  SSDT-PNLF.aml
  SSDT-SBUS-MCHC.aml
  SSDT-USB-Reset.aml
  SSDT-USBX.aml
Drivers/
  HfsPlus.efi
  OpenRuntime.efi
Kexts/
  ...
```

## ACPI

### SSDT-HPET.aml

### SSDT-PLUG-ALT.aml

### SSDT-PNLF.aml

### SSDT-SBUS-MCHC.aml

### SSDT-USB-Reset.aml

### SSDT-USBX.aml

### Kexts

| Kext | Status | Purpose |
|---|:---:|---|
| `IntelMausiEthernet.kext` | Enabled | Intel Ethernet support |
| `Lilu.kext` | Enabled | Kernel patching framework |
| `NVMeFix.kext` | Enabled | NVMe compatibility fixes |
| `RestrictEvents.kext` | Enabled | macOS event corrections |
| `VirtualSMC.kext` | Enabled | Apple SMC emulation |
| `VoodooPS2Controller.kext` | Enabled | PS/2 controller support |
| `VoodooPS2Keyboard.kext` | Enabled | Keyboard support |
| `VoodooPS2Trackpad.kext` | Enabled | Trackpad fallback support |
| `VoodooPS2Mouse.kext` | Disabled | PS/2 mouse plugin |
| `VoodooInput.kext` | Disabled | PS/2 input plugin |
| `VoodooRMI.kext` | Enabled | Synaptics RMI support |
| `RMII2C.kext` | Enabled | RMI I2C transport |
| `RMISMBus.kext` | Enabled | RMI SMBus transport |
| `VoodooInput.kext` | Enabled | RMI input layer |
| `CpuTscSync.kext` | Enabled | TSC synchronization |
| `ECEnabler.kext` | Enabled | Embedded controller support |
| `SMCBatteryManager.kext` | Enabled | Battery data |
| `SMCProcessor.kext` | Enabled | CPU sensor data |
| `SMCSuperIO.kext` | Enabled | Super I/O sensors |
| `AppleALC.kext` | Enabled | Audio codec support |
| `IntelBluetoothFirmware.kext` | Enabled | Intel Bluetooth firmware |
| `IntelBTPatcher.kext` | Enabled | Bluetooth patching |
| `BlueToolFixup.kext` | Enabled | macOS Bluetooth fixes |
| `itlwm.kext` | Enabled | Intel Wi-Fi support |
| `BrightnessKeys.kext` | Enabled | Brightness hotkeys |
| `USBMap.kext` | Enabled | USB port mapping |
| `VoodooPS2Controller` plugins | Mixed | Nested input support |
| `VoodooRMI` plugins | Mixed | Nested RMI support |

`VoodooRMI` uses two transport plugins.  
Its `VoodooInput` plugin is enabled.  

`VoodooPS2Controller` has selective plugins.  
Mouse support remains disabled.  
Its standalone `VoodooInput` remains disabled.  

## Important configuration

### Booter

`ProtectMemoryRegions = True` is critical.  


### Boot arguments

```text
-igfxnotelemetryload revpatch=sbvmm -no_compat_check -keepsyms=1
```

## Coreboot and EDK2

Keep `SecureBootModel` disabled.  
Keep `ProtectMemoryRegions` enabled.

## EFI ACPI Summary

| Component | Key item | Role |
|---|---|---|
| ACPI | `SSDT-PLUG-ALT.aml` | CPU plugin objects |
| ACPI | `SSDT-HPET.aml` | HPET compatibility |
| ACPI | `SSDT-PNLF.aml` | Backlight device |
| ACPI | `SSDT-SBUS-MCHC.aml` | SMBus devices |
| ACPI | `SSDT-USB-Reset.aml` | USB RHUB handling |
| ACPI | `SSDT-USBX.aml` | USB power properties |
| Graphics | `WhateverGreen.kext` | Intel graphics support |
| Audio | `AppleALC.kext` | Audio codec support |
| Wi-Fi | `itlwm.kext` | Intel wireless |
| Bluetooth | Intel BT kext set | Bluetooth support |
| Input | `VoodooRMI.kext` | RMI trackpad |
| Input | `VoodooPS2Controller.kext` | PS/2 input |
| USB | `USBMap.kext` | Port mapping |
| Battery | `SMCBatteryManager.kext` | Battery data |
| Sensors | `SMCProcessor.kext` | CPU sensors |
| Sensors | `SMCSuperIO.kext` | Super I/O sensors |
| Firmware | `HfsPlus.efi` | HFS+ access |
| Firmware | `OpenRuntime.efi` | Runtime services |
| SMBIOS | `MacBookPro15,4` | Platform identity |

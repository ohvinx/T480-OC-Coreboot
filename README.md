# OpenCore ThinkPad T480

OpenCore configuration for Lenovo ThinkPad T480.  
OpenCore version: **1.0.7**.

### Firmware

Coreboot firmware is required here.  
MrChromebox EDK2 is also required.  

`SecureBootModel` is disabled here.  
MrChromebox EDK2 requires this setting.  

`ProtectMemoryRegions` is enabled here.  
This setup requires that setting.  

## macOS

### macOS Tahoe

## What works?

### macOS

- [x] macOS Tahoe

### Hardware

- [x] Intel UHD Graphics 620
- [x] NVIDIA MX150 disabled
- [x] NVMe support
- [x] USB mapping
- [x] Ethernet kext loaded
- [x] Wi-Fi kext loaded
- [x] Bluetooth stack loaded
- [x] Trackpad stack loaded
- [x] Keyboard stack loaded
- [x] Audio kext loaded
- [x] Battery management loaded
- [x] Sensors kexts loaded
- [ ] 4K 60 Hz DisplayPort

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
- [ ] 4K 60 Hz DisplayPort

## Problems

### DisplayPort

4K 60 Hz output does not work.  
See **Known Limitations** below.

### MX150

The NVIDIA MX150 stays disabled.  
Intel UHD 620 remains the active GPU.

### Firmware compatibility

Secure Boot stays disabled here.  
ProtectMemoryRegions stays enabled here.  

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
| **EDK2** | MrChromebox EDK2 fork |
| **OpenCore** | 1.0.7 |
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
This setup needs memory protection enabled.  

These settings support this firmware chain.

### Boot arguments

```text
-igfxnotelemetryload revpatch=sbvmm -no_compat_check alcid=86 keepsyms=1 swd_panic=1 igfxonln=1
```

`alcid=86` selects audio layout 86.  
`igfxonln=1` affects Intel connector behavior.  
`-no_compat_check` bypasses compatibility checking.  
`-igfxnotelemetryload` changes Intel telemetry loading.

### Security

```text
SecureBootModel = Disabled
```

This value is required here.  
MrChromebox EDK2 requires Secure Boot disabled.  

`Vault` remains optional.  
`ScanPolicy` is unrestricted at `0`.

### SMBIOS

| Field | Value |
|---|---|
| Product | `MacBookPro15,4` |
| Serial | `FVFCTSYBL40Y` |
| Board Serial | `FVF910701CD00008C` |
| UUID | `C39501A5-7523-45B4-83E7-78C308480793` |
| ROM | `00:0D:93:67:42:06` |

### UEFI

| Driver |
|---|---|
| `HfsPlus.efi` |
| `OpenRuntime.efi` |

`DisableSecurityPolicy = True` is enabled.  

## Coreboot and EDK2

Keep `SecureBootModel` disabled.  
Keep `ProtectMemoryRegions` enabled.

## GPU configuration

Intel UHD 620 is active.  
The MX150 remains disabled.  

## Known Limitations

- NVIDIA MX150 is disabled.
- **4K 60 Hz over DisplayPort does not work.**
- `SecureBootModel` is disabled.
- MrChromebox EDK2 requires this.
- `ProtectMemoryRegions` is enabled.

## Tested Configuration

| Component | Tested value |
|---|---|
| Laptop | Lenovo ThinkPad T480 |
| CPU | Intel Core i5-8350U |
| RAM | 32 GB |
| iGPU | Intel UHD Graphics 620 |
| dGPU | NVIDIA MX150 disabled |
| Firmware | Coreboot |
| EDK2 | MrChromebox EDK2 fork |
| OpenCore | 1.0.7 |
| macOS | Tahoe |

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
| Critical | `SecureBootModel` | Disabled |
| Critical | `ProtectMemoryRegions` | True |
| SMBIOS | `MacBookPro15,4` | Platform identity |

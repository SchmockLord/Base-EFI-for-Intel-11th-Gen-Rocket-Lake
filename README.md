# Base EFI for Intel 11th Gen Rocket Lake
 Base EFI for Intel 11th Gen Rocket Lake

# BIOS Settings

- First: **Load optimized defaults**
- RAM: Default settings recommended, no XMP Profile
- in general no Overclocking is recommended

## Disable

- **Internal Graphics**, no iGPU support in macOS since 11th Gen Intel
- **Fast Boot**
- **Secure Boot** (see section Secure Boot if you want to enable it)
- USB-Settings: Legacy USB Support
- VT-D (can be enabled if you set DisableIoMapper to YES)
- Compatibility Support Module (CSM).
- Thunderbolt (For initial install, as Thunderbolt can cause issues if not setup correctly)
- Intel SGX (mostly in CPU settings)
- Intel Platform Trust (mostly in CPU settings)
- CFG Lock (MSR 0xE2 write protection) This must be off, if you can't find the option then ENABLE AppleXcpmCfgLock. Your hack will not boot with CFG-Lock enabled.


## Enable

- VT-X or VT-D
- Above 4G decoding. This must be on, if you can't find the option then add npci=0x2000 to boot-args.
Do not have both this option and npci on boot-args enabled at the same time.
When enabling Above4G, Resizable BAR Support may become an available on some motherboards. 
- USB Settings: EHCI Hand-off
- USB-Settings: XHCI Hand-off
- OS type: Windows 8.1/10 UEFI Mode or OTHER
- SATA Mode: AHCI

# ACPI

:----|:----
[SSDT-RHUB.aml](https://dortania.github.io/Getting-Started-With-ACPI/Universal/rhub-methods/prebuilt.html)|If you experience issues with USB when booting macOS, try this AML to force a USB reset. But USB issues probably result from a faulty USB-port mapping.
[SSDT-Thunderbolt4.aml](https://github.com/acidanthera/VirtualSMC/releases)|Used for monitoring CPU temperature, doesn't work on AMD CPU based systems.

# Kexts

# DeviceProperties

# USB

# Audio

# Ethernet

# Thunderbolt

## BIOS Settings for Thunderbolt:
  - Discrete Thunderbolt Support: Enabled
  - Wake From Thunderbolt Devices: Disabled
  - Native OS security for TBT: Disabled

  - Discrete Thunderbolt Configuration:
   - Thunderbolt USB Support: Disabled
   - Thunderbolt Boot Support: Disabled
   - Titan Ridge Workaround for OSUP: Disabled
   - Tbt Dynamic AC/DC L1: Disabled
   - GPIO3 Force Pwr: Enabled
   - Wait time in ms after applying Force Pwr: 200
   - GPIO filter: Enabled

   - DTBT Controller x Configuration: all Settings in this Submenu BIOS-Default

# Wifi/Bluetooth

# Secure Boot

To get Secure Boot running, you need to set a "Secure Boot Model" that is fitting your SMBIOS.

See here: https://dortania.github.io/OpenCore-Post-Install/universal/security/applesecureboot.html#securebootmodel

And you need to enroll all .efi files from you EFI-folder into your BIOS-Secure Boot options.

This whitelists the checksums of all .efi files allowed to be booted from. 

So every time you exchange any of the .efi files, you need to enroll them again. E.g. when you update OC.


**BIOS-Settings for Secure Boot**:
  - Set **Secure Boot** to Enabled
  - Set **Secure Boot Mode** to Custom
  - Go to **Key Management** and then unroll all .efi files in your EFI-folder. This is very different on every board. On Gigabyte this is named **Enroll EFI** and then you have to search for your boot-stick and add all *.efi Files one by one: BOOTX64.efi, all drivers(OpenRunTime.efi, OpenHFSPlus.efi, OpenCanopy.efi), OpenCore.EFI
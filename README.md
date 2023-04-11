# Opencore 0.9.1 Hackintosh intel i5-13600k, ASUS Z790P WIFI RADEON 6800XT 

- Asus prime Z790-P WIFI
- Corsair RM1000E
- Kingstom Fury Beast 2x16 DDR5 5600MHZ
- Corsair WAK Cooling Hydro H100i ( controlled with Liquidctl --> https://github.com/liquidctl/liquidctl )
- Intel i5-13600K
- SSD Kingston 2000 GB FURY Renegade PCIe 4.0 NVMe M.2 SSD
- Sapphire Radeon Nitro+ RX 6800 XT OC SE


**Kexts used:**

AppleALC.kext
BlueToolFixup.kext
Lilu.kext
LucyRTL8125Ethernet.kext
NVMeFix.kext
RadeonSensor.kext
SMCProcessor.kext
SMCRadeonGPU.kext
SMCSuperIO.kext
USBToolBox.kext
UTBMap.kext
VirtualSMC.kext
WhateverGreen.kext


**Dumping your DSDT in Windows Environment**

https://acpica.org/downloads/binary-tools

Open the CMD in the directory where the ACPI Tools was extracted. (Command Prompt) in Administrator Mode:

path/to/acpidump.exe -b -n DSDT -z
move dsdt.dat DSDT.aml
Decompile DSDT.aml:

path/to/iasl.exe path/to/DSDT.aml
File DSDT.dsl will generated. Use this for generate YOUR ACPI Patches.

Compile DSDT.dsl:

path/to/iasl.exe path/to/DSDT.dsl
File APCPI_FILE_PATCHED.aml will generated.

#SSDTIME
![A simple tool designed to make creating SSDTs simple. Supports macOS, Linux and Windows](https://github.com/corpnewt/SSDTTime)

Supported SSDTs:

- SSDT-HPET
Patches out IRQ conflicts
- SSDT-EC
OS-aware fake EC (laptop and desktop variants)
- SSDT-USBX
Provides generic USB power properties
- SSDT-PLUG
Sets plugin-type = 1 on CPU0/PR00
- SSDT-PMC
Adds missing PMCR device for native 300-series NVRAM
- SSDT-AWAC
Disables AWAC clock, and enables (or fakes) RTC as needed
- SSDT-USB-Reset
Returns a zero status for detected root hubs to allow hardware querying
- SSDT-Bridge
Create missing PCI bridges for passed device path
- SSDT-PNLF
Sets up a PNLF device for laptop backlight control
- SSDT-XOSI
_OSI rename and patch to return true for a range of Windows versions - also checks for OSID
Additionally on Linux and Windows the tool can be used to dump the system DSDT.


# **Special notes**

USB port mapping is REQUIRED.
XhciPortLimit - Needed DISABLE if you use Big Sur 11.3+.
Please Mapping USB in macOS Catalina before install Big Sur or Newer for best results.
You can use USBMap.command Utility - USBMap.
AppleXcpmCfgLock - Please ENABLE if you cannot disableCFG-Lock in BIOS.
Does NOT SUPPORT iGPU in 13th Gen.
You NEED dGPU (dedicated/discrete GPU (eg. RX 560, 570, 580, 590, RX 5700 XT, etc).
SetupVirtualMap - Please ENABLE if you stuck in Early boot.

# **GPU-Specific boot-args**

Parameter	Description
agdpmod=pikera	Used for disabling board ID checks on Navi GPUs(RX 5000 series & RX 6000 series), without this you'll get a black screen.
Don't use if you don't have Navi (ie. Polaris and Vega cards shouldn't use this).

# **BIOS Settings**

## Disable

Fast Boot
Secure Boot
Serial/COM Port
Parallel Port
VT-d (can be enabled if you set DisableIoMapper to YES)
Compatibility Support Module (CSM).
Thunderbolt(For initial install, as Thunderbolt can cause issues if not setup correctly)
Intel SGX
Intel Platform Trust
CFG Lock (MSR 0xE2 write protection)
This must be off, if you can't find the option then ENABLE AppleXcpmCfgLock.
Your hack will not boot with CFG-Lock enabled.

## Enable

VT-x
Above 4G decoding.
This must be on, if you can't find the option then add npci=0x2000 to boot-args.
Do not have both this option and npci on boot-args enabled at the same time.
When enabling Above4G, Resizable BAR Support may become an available on some motherboards. Please ensure this is DISABLED instead of set to Auto.
Hyper-Threading
Execute Disable Bit
EHCI/XHCI Hand-off
OS type: Windows 8.1/10 UEFI Mode
SATA Mode: AHCI

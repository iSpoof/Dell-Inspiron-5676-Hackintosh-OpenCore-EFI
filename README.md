# Dell Inspiron 5676 - Ryzentosh/Hackintosh

<img src="https://avallax.com/wp-content/uploads/2019/11/Dell-Inspiron-5676_4.jpg" width="15%"></img>


## <b>My System Configuration</b>

| Processor | AMD Ryzen 7 2700 or 2700X (8-Cores, 16-Threads)	| Graphics       | AMD Radeon RX 580 (4GB - GDDR5) (Polaris 10)	|
| Memory    | 32GB (2x16GB) DDR4 @ 2400MHz			| OS Disk        | Inland 240GB SSD				|
| Audio     | Realtek ALC898					| WiFi/Bluetooth | Qualcomm Atheros QCA9377      		|
| Network   | Realtek RTL8168/8111				| Chipset        | X370 Series                          	|

## <b>Compatible Dell Systems</b>

| Model			| Processor									| USB Ports (Front & Back)							|
|-----------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| Inspiron 5675 [v1]*	| AMD A10-9700, AMD A12-9800							| x4 USB 3.0, x4 USB 2.0, SD Reader, x2 PS2 (Mouse/Keyboard)			|
| Inspiron 5675 [v2]*	| AMD Ryzen 3 1200, AMD Ryzen 5 1400, AMD Ryzen 5 1600X, AMD Ryzen 7 1700X	| x6 USB 3.0, x1 Type-C 3.0, x4 USB 2.0, SD Reader, x2 PS2 (Mouse/Keyboard)	|
| Inspiron 5676 [v1]**	| AMD Ryzen 5 1400, AMD Ryzen 7 2700, AMD Ryzen 7 2700X				| x5 USB 3.0, x1 Type-C 3.0, x4 USB 2.0, x2 PS2 (Mouse/Keyboard)		|
| Inspiron 5676 [v2]*** | AMD Ryzen 5 1400, AMD Ryzen 7 2700, AMD Ryzen 7 2700X				| X6 USB 3.0, x4 USB 2.0, x2 PS2 (Mouse/Keyboard)				|
#### EFI is compatible but requires minor changes (Core count in AMD Ryzen Patches + USB Mapping).
#### Inspiron 5675 [v2] may have other Zen processors (If customized/purchased from Dell).
#### Inspiron 5676 [v1 + v2] may have other Zen/Zen+ processors (If customized/purchased from Dell).
#### * Includes Slim ODD + 2.5 Drive Bay
#### ** Includes Slim ODD
#### *** Uses same internal chasis as other varients; Front IO + Plastic Cover replaceable with other varients (Has same unused internal ports). 

## Ryzentosh/Hackintosh Info

| OpenCore  | [v0.9.3](https://github.com/acidanthera/OpenCorePkg/releases)				| macOS 	 | 13.0	(Ventura)                              	|

## 

-----------------------------------------------------------------------------------------------------------------

[ Manufacturer ]          Dell
[ Model ]                 Inspiron 5676
[ CPU ]                   AMD Ryzen 7 2700 (8-Core)                     +    2700X (2nd Desktop PC)
[ GPU ]                   AMD Radeon RX 580 (4 GB) (Polaris 10)              
[ Motherboard ]           Promontory X370                                    
[ Network ]               Realtek RTL8168/8111                               
[ Audio ]                 Realtek ALC898                                     [ UNTESTED ]
[ Wifi/BT ]               Qualcomm Atheros QCA9377                           [ INCOMPATIBLE ]
[ Storage ]               NVMe - Crucial P3 Plus - 2TB                  +    Samsung PM961 - 512GB (2nd PC)
	                  SATA - Intel SSD Pro 1500 Series - 180GB      +    Inland - 240GB (2nd PC)
[ Memory ]                2x8GB @ 2400MHz DDR4                          +    2x16GB @ 2400MHz DDR4 (2nd PC)

[ Connection Type ]       USB (HID Compliant)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

[ USB INSTALLER ]
	|
	|_____ com.apple.recovery.boot
	|		|_____ (macOS - Ventura)
	|
	|_____ EFI
		|_____ BOOT
		|	|_____ `BOOTx64.efi`
		|
		|_____ OC
			|_____ ACPI
			|	|_____ `SSDT-EC-USBX-DESKTOP.aml`
			|
			|_____ DRIVERS
		 	|	|_____ `HfsPlus.efi`
			|	|_____ `OpenRuntime.efi`
			|
			|_____ KEXTS
			|	|_____ `AMDRyzenCPUPowerManagement.kext`
			|	|_____ `AppleALC.kext`
			|	|_____ `AppleMCEReporterDisabler.kext`
			|	|_____ `Lilu.kext`
			|	|_____ `RadeonSenson.kext`
			|	|_____ `RealtekRTL8111.kext`
			|	|_____ `SMCAMDProcessor.kext`
			|	|_____ `SMCRadeonGPU.kext`
			|	|_____ `USBToolBox.kext`
			|	|_____ `UTBMap.kext`****
			|	|_____ `VirtualSMC.kext`
			|	|_____ `WhateverGreen.kext`
			|
			|_____ RESOURCES
			|	|_____ [ NO CHANGES MADE FROM 'sample.plist' ]
			|
			|_____ TOOLS
			|	|_____ `OpenShell.efi`
			|
			|_____ `config.plist`
			|_____ `OpenCore.efi`

#### **** DO NOT USE pre-built `UTBMap.kext` unless your USB ports match 'Inspiron 5676 [v2]' mentioned in 'Compatible Dell Systems'.
#### **** Building your own [UTBMap.kext](https://github.com/USBToolBox/tool) is recomended.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

[ config.plist ] = Open then Clean Snapshot if new ACPI/DRIVERS/KEXTS added.

[ ACPI ]                         Add > SSDT-EC-USBX

[ Booter ]                    Quirks > DevirtualiseMmio = False
                                       EnableWriteUnprotector = True
                                       RebuildAppleMemoryMap = False
                                       ResizeAppleGpuBars = -1
                                       SetupVirtualMap = True
                                       SyncRuntimePermissions = False

[ DeviceProperties ]             Add > DELETE "PciRoot(0x0)/Pci(0x1b,0x0)"

[ Kernal ]                   Emulate > DummyPowerManagement = True
                               Patch > ADD PATCHES FROM "patches.plist"
                                       & modify patches named
                                       "algrey - Force cpuid_cores_per_package"
                                       with (08).
                               Quirk > PanicNoKextDump = True
                                       PowerTimeoutKernalPanic = True
                                       ProvideCurrentCpuInfo = True
                                       XhciPortLimit = False (macOS 13 - Ventura)

[ Misc ]                        Boot > HideAuxiliary = True
                               Debug > AppleDebug = True
                                       ApplePanic = True
                                       DisableWatchDog = True
                            Security > AllowSetDefault = True
                                       BlacklistAppleUpdate = True
                                       ScanPolicy = 0
                                       SecureBootModel = Default
                                       Vault = Optional

[ NVRAM ]                        Add > boot-args = -v keepsyms=1 npci=0x3000
                                       prev-lang:kbd = en-US:0 (Change type from 'Data' to 'String')
                              Delete > WriteFlash = True

[ PlatformInfo ]                       SystemProductName = CUSTOM
                                       SystemSerialNumber = CUSTOM
                                       MLB = CUSTOM
                                       SystemUUID = CUSTOM
                                       ROM = CUSTOM

[ UEFI ]                     Drivers > HfsPlus.efi
                                       OpenRuntime.efi
                              Quirks > UnlockFsConnect = False
                                       ReleaseUSBOwnership = True

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

The following are issue I have & don't know how to fix.

* Unable to 'ShutDown' when on Windows.
	- Windows ends up on Windows login screen, as if it 'Restarted'.
	- Do note, power to computer did not stop (indicated by desktop built-in-lights).
* macOS Ventura Time Zone unable to be changed.
	- It's locked to Pacific Time (UTC-8).
	- I use Eastern Time (UTC-5)
* Windows Clock gets changed macOS was used.
	- Might be caused from Dual booting each OS (Windows & macOS) on seperate SSDs.
	- Windows Clock easily fixed with 'Sync Now' option in settings.
* Unable to boot into macOS without USB install drive.
	- Possible user error.
	- Mounted macOS drive EFI (SSD ?), then placed EFI content from USB to 'mounted SSD EFI'.
	- Pretty sure I did somehting wrong.
* Disabled/Non-working WIFI, Bluetooth, AirDrop, StageManager, & Screen Mirroing.
	- Possible cause is the incompatible WIFI/BT card (Qualcomm Atheros QCA9377).
* Audio
	- Untested (Forgot to test/montior muted by mistake)
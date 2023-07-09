# Dell Inspiron 5676 - Ryzentosh/Hackintosh

<img src="https://avallax.com/wp-content/uploads/2019/11/Dell-Inspiron-5676_4.jpg" width="15%"></img>

## <b>My System Configuration</b>
| Type		| Name								|
|---------------|---------------------------------------------------------------|
| OpenCore	| [v0.9.3](https://github.com/acidanthera/OpenCorePkg/releases)	|
| macOS		| 13.0 (Ventura)						|
| Desktop Model	| Dell Inspiron 5676						|
| Processor	| AMD Ryzen 7 2700 or 2700X (8-Cores, 16-Threads)		|
| Graphics	| AMD Radeon RX 580 (4GB - GDDR5) (Polaris 10)			|
| Memory	| 16GB (2x8GB) DDR4 @ 2400MHz					|
| OS Disk	| Inland 240GB SSD (SATA III)					|
| Audio		| Realtek ALC898						|
| WiFi/Bluetooth| Qualcomm Atheros QCA9377					|
| Network	| Realtek RTL8168/8111						|
| Chipset	| X370 Series							|

## <b>Compatible Dell Systems</b>
| Model			| Processor									| USB Ports (Front & Back)							|
|-----------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| ~~Inspiron 5675 [v1]~~| ~~AMD A10-9700, AMD A12-9800~~						| x4 USB 3.0, x4 USB 2.0, SD Reader, x2 PS2 (Mouse/Keyboard)			|
| Inspiron 5675 [v2]	| AMD Ryzen 3 1200, AMD Ryzen 5 1400, AMD Ryzen 5 1600X, AMD Ryzen 7 1700X	| x6 USB 3.0, x1 Type-C 3.0, x4 USB 2.0, SD Reader, x2 PS2 (Mouse/Keyboard)	|
| Inspiron 5676 [v1]	| AMD Ryzen 5 1400, AMD Ryzen 7 2700, AMD Ryzen 7 2700X				| x5 USB 3.0, x1 Type-C 3.0, x4 USB 2.0, x2 PS2 (Mouse/Keyboard)		|
| Inspiron 5676 [v2]	| AMD Ryzen 5 1400, AMD Ryzen 7 2700, AMD Ryzen 7 2700X				| X6 USB 3.0, x4 USB 2.0, x2 PS2 (Mouse/Keyboard)				|
>Requires modifying EFI based on [Processor](https://github.com/AMD-OSX/AMD_Vanilla), [USB Ports](https://github.com/USBToolBox/tool).

>Inspiron 5675 [v1] removed due to APUs not being supported, but small chance USB Port configuration may have been used with Inspiron 5675 [v2], & Inspiron 5676 [v1] + [v2].

## BIOS Settings
| Setting	| Status	|
|---------------|---------------|
| Fast Boot	| Minimal	|
| Secure Boot	| Disabled	|

## Installer Configuration
<details><summary><strong>USB Directory</strong></summary>
 
```bash
[ USB Installer ]
	|
	|_____ com.apple.recovery.boot
	|		|_____ `BaseSystem.chunklist`
	|		|_____ `BaseSystem.dmg`
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
			|	|_____ `UTBMap.kext`
			|	|_____ `VirtualSMC.kext`
			|	|_____ `WhateverGreen.kext`
			|
			|_____ RESOURCES
			|	|_____ [ NO CHANGES MADE FROM 'sample.plist' ]
			|	|
			|_____ TOOLS
			|	|_____ `OpenShell.efi`
			|
			|_____ `config.plist`
			|_____ `OpenCore.efi`
```
</details>

<details><summary><strong>config.plist</strong></summary>

| Location		| Name												| Value										|
|-----------------------|-----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| Booter > Quirks	| DevirtualiseMmio										| `False`									|
| 			| EnableWriteUnprotector									| `True`									|
| 			| RebuildAppleMemoryMap										| `False`									|
| 			| ResizeAppleGpuBars										| `-1`										|
| 			| SetupVirtualMap										| `True`									|
| 			| SyncRuntimePermissions									| `False`									|
| Kernel > Emulate	| DummyPowerManagement										| `True`									|
| Kernel > Patch	| [More Info Here](https://dortania.github.io/OpenCore-Install-Guide/AMD/zen.html#patch-2)	| 										|
| Kernel > Quirks	| PanicNoKextDump										| `True`									|
| 			| PowerTimeoutKernalPanic									| `True`									|
| 			| ProvideCurrentCpuInfo										| `True`									|
| 			| XhciPortLimit											| `False`									|
| Misc > Boot		| HideAuxiliary											| `True`									|
| Misc > Debug		| AppleDebug											| `True`									|
| 			| ApplePanic											| `True`									|
| 			| DisableWatchDog										| `True`									|
| Misc > Security	| AllowSetDefault										| `True`									|
| 			| BlacklistAppleUpdate										| `True`									|
| 			| ScanPolicy											| `0`										|
| 			| SecureBootModel										| `Default`									|
| 			| Vault												| `Optional`									|
| NVRAM > Add		| boot-args											| `-v keepsyms=1 npci=0x3000`							|
| 			| prev-lang:kbd											| `en-US:0`									|
| NVRAM			| WriteFlash											| `True`									|
| PlatformInfo > Generic| SystemProductName										| [Generate your own SMBIOS](https://github.com/corpnewt/GenSMBIOS)		|
| 			| SystemSerialNumber										| [Generate your own SMBIOS](https://github.com/corpnewt/GenSMBIOS)		|
| 			| MLB												| [Generate your own SMBIOS](https://github.com/corpnewt/GenSMBIOS)		|
| 			| SystemUUID											| [Generate your own SMBIOS](https://github.com/corpnewt/GenSMBIOS)		|
| 			| ROM												| [Generate your own SMBIOS](https://github.com/corpnewt/GenSMBIOS)		|
| UEFI > Quirks		| UnlockFsConnect										| `False`									|
| 			| ReleaseUSBOwnership										| `True`									|
>Open (Ctrl + O) then Clean Snapshot (Ctrl + Shift + R) if new ACPI (.aml), DRIVERS (.efi), or KEXTS (.kext) added.
</details>

>The provided EFI was made for an Inspiron 5676 [v2] with AMD Ryzen 7 2700 or 2700X. If you have a different system configuration, you will need to modify the provided EFI accordingly.
</details>

## Known Issues
* Unable to 'ShutDown' when on Windows after macOS was installed.
	- Attempting to shutdown the computer while on Windows takes me back to login screen, as if it 'Restarted' or 'Signed-Out'.
	- Do note, power to computer did not stop (indicated by computers built-in lights).
	- FIX: Go to 'Control Panel > Hardware and Sound > Power Options > System Settings > DISABLE 'Turn on fast startup'
* macOS Ventura Time Zone unable to be changed.
	- It's locked to Pacific Time (UTC-8).
	- I use Eastern Time (UTC-5)
* Windows Clock gets changed macOS was used.
	- Might be caused from Dual booting each OS (Windows & macOS) on seperate SSDs.
	- Windows Clock easily fixed with 'Sync Now' option in settings.
* Unable to boot into macOS without USB install drive.
	- Possible user error.
	- Mounted macOS drive EFI (OS Drive - SSD), then transfered USB EFI content to mounted SSD EFI.
	- Pretty sure I did somehting wrong somewhere along the install process or just this mounting part.
* Non-working WIFI, Bluetooth, AirDrop, StageManager, & Screen Mirroing.
	- Possible cause is the incompatible WIFI/BT card I have - Qualcomm Atheros QCA9377.
* Audio
	- Untested (Forgot to test/montior muted by mistake)

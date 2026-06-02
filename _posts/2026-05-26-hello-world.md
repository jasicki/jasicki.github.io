---
title: "Downgrading iPhone X to iOS 14.3"
date: 2026-05-27 12:00:00 +0200
description: "A guide for tether downgrading the iPhone X from latest iOS 16.7.16 to iOS 14.3 or other supported"
---

## Disclaimer

I am not responsible for bricking your devices caused by following this guide. Please proceed with caution and at your own risk.

## Credits

* [pwnerblu](https://github.com/pwnerblu/surrealra1n/commits?author=pwnerblu) - For the surrealra1n script.
* [checkm8 exploit](https://github.com/axi0mX/ipwndfu) - For the bootrom vulnerability.
* [libimobiledevice](https://github.com/libimobiledevice) - For irecovery.
* [LukeZGD](https://github.com/LukeZGD) - For Legacy iOS Kit

## Requirements

* A Mac running macOS.
* A Lightning to USB-A cable (USB-C also works but more trouble with entering DFU).
* [iOS 14.3 ipsw](https://updates.cdn-apple.com/2020WinterFCS/fullrestores/001-87865/458334F5-D8E1-498A-A9FD-08BBD20FE007/iPhone10,3,iPhone10,6_14.3_18C66_Restore.ipsw) file as your target system and latest signed iOS 16.x.x ipsw
(_You can get these from [The Apple Wiki](https://theapplewiki.com/wiki/Firmware)_)
* Homebrew installed on your Mac.
* [SSHRD Script](https://github.com/verygenericname/SSHRD_Script) for SSH ramdisk to bypass activation error.
* _alternative for ssh_ [Legacy iOS Kit](https://github.com/LukeZGD/Legacy-iOS-Kit)

## Preparation

1. Open Terminal and install dependencies:
```bash
   xcode-select --install
   brew install libimobiledevice
   brew install libirecovery
   brew install binutils
   ```
2. Download the required downgrade script repository ([pwnerblu/surrealra1n](https://github.com/pwnerblu/surrealra1n)):
```bash
   git clone https://github.com/pwnerblu/surrealra1n.git
   cd surrealra1n
   ```
3. Set permissions for `surrealra1n`
```bash
   chmod +x surrealra1n.sh
   ```

## Creating the Custom IPSW

1. Move your downloaded iOS 14.3 IPSW into the `surrealra1n` folder.
2. Run the script. This will create a custom restore ramdisk with patched ASR and ipsw:
```bash
   ./surrealra1n.sh --make_custom_ipsw iPhone10,6_14.3_18C66_Restore.ipsw [latest iOS 16 IPSW] 14.3
   ```

## Restoring the Device

1. Connect your iPhone X and put it into DFU mode:
   * Press and quickly release Volume Up.
   * Press and quickly release Volume Down.
   * Press and hold the Side button until the screen goes black.
   * When goes black release all buttons and imeditialy quickly press volume down and Side button for 3 seconds.
   * Release the Side button but keep holding Volume Down for another 10 seconds and PC recognization.

2. Flash the custom IPSW using the restore script:
```bash
   ./surrealra1n.sh --restore 14.3
   ```
This may take a while.

3. Now device should enter recovery mode.

## Tethered Booting

Since this is a tethered downgrade, your device will not boot on its own. You must use your computer to boot it every time it restarts.

1. Put the device back into DFU mode as mentioned.
2. Run the boot script:
```bash
   ./surrealra1n.sh --boot 14.3
   ```
3. Your iPhone X should now boot into iOS 14.3.

## Bypassing activation error

blablabla bedzie pozniej


Done!

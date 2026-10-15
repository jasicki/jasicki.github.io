---
title: "iPad Air 2 nonUI iOS 9 internal guide"
date: 2026-10-14 23:00:00 +0200
description: "Complete manual guide for tether booting nonUI system on iPad Air 2"
---

## Disclaimer

I am not responsible for bricking your devices, data loss, or any other damage caused by following this guide. Manually modifying partition tables and bootchains carries risks. Please proceed with caution and at your own risk.

## Credits

* [axi0mX](https://github.com/axi0mX/ipwndfu) - For the checkm8 bootrom exploit.
* [tihmstar](https://github.com/tihmstar/iBoot64Patcher) - For `iBoot64Patcher`.
* [mineek](https://github.com/mineek/seprmvr64) - For `seprmvr64` and SEP bypass research.
* [verygenericname](https://github.com/verygenericname/SSHRD_Script) - For the SSH Ramdisk creation tools.

---

## 1. Requirements & Prerequisites

* A computer running **macOS**.
* A Lightning to USB-A cable.
* Target **nonui ios 9 IPSW** and an **iOS 9.0.2 IPSW** (for ramdisk/bootloader components) downloaded from [ipsw.me](https://ipsw.me).
* Essential command-line tools installed:
  * `ipwndfu` / `gaster`
  * `img4tool` / `image3maker`
  * `iBoot64Patcher`
  * `seprmvr64` / `Kernel64Patcher`
  * `irecovery` & `libimobiledevice` (`iproxy`)

### Installing Dependencies (macOS)

1. Open Terminal and install the required packages via Homebrew:
   ```bash
   brew install libimobiledevice libirecovery cmake openssl zlib
   ```
2. Create a working directory for all your downgrade files:
   ```bash
   mkdir ~/ios7-downgrade
   cd ~/ios7-downgrade
   ```
3. Extract your downloaded target iOS 7 IPSW (which is just a ZIP archive):
   ```bash
   unzip iPhone6,2_7.1.2_11D257_Restore.ipsw -d extracted_ipsw/
   ```

---

## 2. Preparing & Patching Boot Files

Because we don't have SHSH blobs for iOS 7 and the modern SEP is incompatible with iOS 7, we must decrypt and patch the entire bootchain (`iBSS`, `iBEC`, `devicetree`, and `kernelcache`). Find the Firmware Keys (IV + Key) for your specific device and iOS version on **The Apple Wiki**.

### Patching iBSS and iBEC

1. Decrypt `iBSS` and `iBEC` using `img4tool` or `xpwntool` with the keys from The Apple Wiki:
   ```bash
   img4tool -e --iv <iBSS_IV> --key <iBSS_KEY> -o iBSS.dec extracted_ipsw/Firmware/dfu/iBSS.n53ap.RELEASE.dfu
   img4tool -e --iv <iBEC_IV> --key <iBEC_KEY> -o iBEC.dec extracted_ipsw/Firmware/dfu/iBEC.n53ap.RELEASE.dfu
   ```
2. Patch signature checks and add custom boot arguments (`-v rd=disk0s1s1 amfi=0xff cs_enforcement_disable=1`) using `iBoot64Patcher`:
   ```bash
   iBoot64Patcher iBSS.dec iBSS.patched
   iBoot64Patcher iBEC.dec iBEC.patched -b "-v rd=disk0s1s1 amfi=0xff cs_enforcement_disable=1"
   ```
3. Pack the patched files back into `.img3` (or `.im4p` depending on your boot setup):
   ```bash
   image3maker -t ibss -f iBSS.patched -o iBSS.img3
   image3maker -t ibec -f iBEC.patched -o iBEC.img3
   ```

### Patching Kernelcache (SEP Bypass)

1. Decrypt the `kernelcache`:
   ```bash
   img4tool -e --iv <KC_IV> --key <KC_KEY> -o kcache.raw extracted_ipsw/kernelcache.release.n53
   ```
2. Run `seprmvr64` and `Kernel64Patcher` to remove SEP requirements and disable Sandbox/AMFI:
   ```bash
   seprmvr64 kcache.raw kcache.sep_patched
   Kernel64Patcher kcache.sep_patched kcache.patched -a
   ```
3. Repack the patched kernelcache:
   ```bash
   image3maker -t krnl -f kcache.patched -o kernelcache.img3
   ```

### Decrypting DeviceTree

1. Decrypt and repack the `DeviceTree` (no binary patching required unless modifying mounts):
   ```bash
   img4tool -e --iv <DT_IV> --key <DT_KEY> -o dtree.raw extracted_ipsw/Firmware/all_flash/all_flash.n53ap.production/DeviceTree.n53ap.img3
   image3maker -t dtre -f dtree.raw -o devicetree.img3
   ```

---

## 3. Booting the SSH Ramdisk

To partition the drive and install HFS+, you need to boot an SSH Ramdisk (preferably an iOS 8 or iOS 9 ramdisk that natively supports `newfs_hfs` and `asr` for HFS+ volumes).

1. Clone and prepare the SSH Ramdisk tool:
   ```bash
   git clone [https://github.com/verygenericname/SSHRD_Script](https://github.com/verygenericname/SSHRD_Script) --recursive
   cd SSHRD_Script
   ```
2. Put your A7 device into **DFU Mode** and exploit it with `gaster` or `ipwndfu`:
   ```bash
   gaster pwn
   gaster reset
   ```
3. Generate and boot the SSH Ramdisk:
   ```bash
   ./sshrd.sh 10.3.3
   ./sshrd.sh boot
   ```
4. Once the device screen shows verbose text and boots into the ramdisk, forward port 22 over USB:
   ```bash
   iproxy 2222 22 &
   ```
5. Connect to the device via SSH (default password is **`alpine`**):
   ```bash
   ssh -p 2222 root@localhost
   ```

---

## 4. Installing the Filesystem

Now that you are inside the SSH Ramdisk, you must format the storage to HFS+ and restore the iOS 7 root filesystem.

### Decrypting RootFS (On Computer)

1. Before uploading, decrypt the main RootFS DMG from the extracted IPSW using `dmg` or `vfdecrypt`:
   ```bash
   vfdecrypt -i extracted_ipsw/058-4236-009.dmg -k <ROOTFS_KEY> -o RootFS.dmg
   ```

### Partitioning & Formatting (On Device via SSH)

1. If you were previously on iOS 10.3.3 (APFS), recreate the standard LwVM / GPT HFS+ layout, then format `disk0s1s1` (System) and `disk0s1s2` (Data):
   ```bash
   newfs_hfs -s -v System -J -b 4096 /dev/disk0s1s1
   newfs_hfs -s -v Data -J -P -b 4096 /dev/disk0s1s2
   ```
2. Mount the newly formatted partitions:
   ```bash
   mount_hfs /dev/disk0s1s1 /mnt1
   mount_hfs /dev/disk0s1s2 /mnt2
   ```

### Restoring RootFS

1. Open a **new terminal window on your computer** and transfer the decrypted `RootFS.dmg` to the device's data partition (`/mnt2`):
   ```bash
   scp -P 2222 ~/ios7-downgrade/RootFS.dmg root@localhost:/mnt2/
   ```
2. Return to your **SSH terminal**, unmount `/mnt1`, and use `asr` to restore the RootFS onto the system partition:
   ```bash
   umount /mnt1
   asr restore --source /mnt2/RootFS.dmg --target /dev/disk0s1s1 --erase --noprompt
   ```
3. Wait for the verification and restore process to complete, then remount `/mnt1` and delete the temporary DMG file:
   ```bash
   mount_hfs /dev/disk0s1s1 /mnt1
   rm /mnt2/RootFS.dmg
   ```

### Post-Restore Patches

1. Move the contents of `/private/var` from the system partition to the data partition so the OS has proper user storage structure:
   ```bash
   cp -rp /mnt1/private/var/* /mnt2/
   ```
2. Edit `/mnt1/etc/fstab` to ensure partitions mount as read-write (`rw`) and remove any `noexec` or SEP keybag constraints:
   ```bash
   sed -i 's/ro/rw/g' /mnt1/etc/fstab
   ```
3. Bypass `Setup.app` (Activation Lock / Unactivated state due to missing SEP & Baseband):
   ```bash
   mv /mnt1/Applications/Setup.app /mnt1/Applications/Setup.app.bak
   ```
4. Unmount both partitions cleanly and reboot the device back into DFU mode:
   ```bash
   umount /mnt1
   umount /mnt2
   reboot
   ```

---

## 5. Tethered Booting

Every time the device turns off or reboots, you will need a computer to send the patched bootchain over USB.

1. Place your device into **DFU Mode**.
2. Exploit the bootrom to enter Pwned DFU:
   ```bash
   gaster pwn
   ```
3. Send the patched boot files in the exact order using `irecovery`:
   ```bash
   irecovery -f ~/ios7-downgrade/iBSS.img3
   irecovery -f ~/ios7-downgrade/iBEC.img3
   irecovery -f ~/ios7-downgrade/devicetree.img3
   irecovery -c "devicetree"
   irecovery -f ~/ios7-downgrade/kernelcache.img3
   irecovery -c "bootx"
   ```
4. Your device screen should light up, display verbose boot lines, and boot straight into the iOS 7 SpringBoard.

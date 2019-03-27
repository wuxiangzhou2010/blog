---
layout: post
title: "z370n-wifi hackintosh build"
date: 2018-12-27 12:00 +0800
categories: hackintosh
published: true
---

This is my first Hackintosh build, before I actually started to buy components, I had done a lot of search ,  mostly on [`tonymacx68`](http://www.tonymacx86.com) and [`hackintosher`](https://hackintosher.com/). This build is mainly for everday use and sometimes a little code development.

 **Components list**

| Component   | Model                         | note          |
| ----------- | ----------------------------- | ------------- |
| CPU         | Intel i3 8100                 | Taobao 659RMB |
| Motherboard | Giga-z370n-wifi itx  Bios: f4 | Xianyu 900RMB |
| RAM         | Crucial DDR4 2666 8G          | Xianyu 265RMB |
| Case        | In Win Chopin                 | Xianyu 400RMB |
| Cooler      | Noctua NH-L9I                 | Taobao 259RMB |
| SSD         | Samsung 840 EVO 120G          | already owned |
| screen      | Dell E2214hv                  | already owned |

total: 2483 RMB(370 USD)

The main installation process is as follows

1. Use a mac to download a macOS installer app from app store, [download link](https://support.apple.com/en-us/HT201372)
2. Use `Disk util` to format a USB drive and flash the OS image to USB
   - select erase: name, `usb`, format `Mac OS Extended (Journaled)`, schema `GUID Partition Map`
   - open a terminal, flash the os image with following command

    ```sh
    sudo /Applications/Install\ macOS\ .app/Contents/Resources/createinstallmedia --volume /Volumes/usb --nointeraction
    ```

3. Mount EFI folder using `clover configurator` and correct the EFI by place the [preconfigured EFI from hackintosher,Mojave-10.14.2-EFI](https://hackintosher.com/wp-content/uploads/Hackintosher-Mojave-10.14.2-EFI.zip)
4. Set proper BIOS options, save your modification.
5. Reboot from USB installer and install MacOS on a SSD

   - you may need to format the disk before install on the drive
   - name: `hackintosh_hd`, format: `Mac OS Extended (Journaled)`, schema: `GUID Partition Map`

6. Put the same EFI folder on the new MacOS

### fix issue

a little adjustment may be needed

- fix blackscreen after wakeup sleep/wake : Boot -> Arguments -> dart=0
- reboot after shutdown: Acpi--> fixshutdown-->checked

### Kexts

kext files are basically the drivers for macOS or the kernel modules for Linux. The word "Kext" is short for Kernel Extension. Kexts are extensions of the macOS  kernel. When you boot up your machine the code contained in these kexts is automatically injected into the operation system.

#### Required

- [FakeSMC](https://bitbucket.org/RehabMan/os-x-fakesmc-kozlek/downloads): FakeSMC is an open source SMC device driver/emulator developed by netkas. This kext is needed to emulate a mac and boot up.
- [Lilu](https://github.com/acidanthera/Lilu/releases) : An open source kernel extension bringing a platform for arbitrary kext, library, and program patching throughout the system for macOS. This kext is need for many other kexs to work.

#### Audio

- [AppleALC](https://github.com/acidanthera/AppleALC/releases/):An open source kernel extension enabling native macOS HD audio for not officially supported codecs without any filesystem modifications.
- [CodecCommander](https://bitbucket.org/RehabMan/os-x-eapd-codec-commander/downloads/)

#### Video

- [WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases):Lilu plugin providing patches to select GPUs on macOS. Requires Lilu 1.2.5 or newer.

#### Network

- [IntelMausiEthernet](https://bitbucket.org/RehabMan/os-x-intel-network/downloads):OS X driver for Intel onboard LAN

### Todo

1. case
  
    Change the case to a larger one, say Inwin 301 matx case. This will make a lot room for other staff like graphic card.

2. Graphic card

    I found amd card, like rx580 work out of the box, As my girl friend want to do photo work like PS and AE, maybe I need to add a graphic card.

3. wifi
  
    I Plan to use an Apple part to get wifi work out of the box. I do a lot of search and found that `bcm94360cs2` with m.2 adapter works fine. However the Inwin itx case is two small to fit the adapter. A larger case is needed.

4. Magic trackpad

    I heard a lot that Apple's trackpad is a must for real Macos experience.

### Reference

- [success-gigabyte-z370n-wifi-i5-8400-nvme-970-evo-mojave-10-14-1-vanilla-hackintosh-deluxe-build](https://hackintosher.com/forums/thread/success-gigabyte-z370n-wifi-i5-8400-nvme-970-evo-mojave-10-14-1-vanilla-hackintosh-deluxe-build.704/)
- [gigabyte-z370n-wifi-itx-hackintosh-guide-4k-htpc-build](https://hackintosher.com/builds/gigabyte-z370n-wifi-itx-hackintosh-guide-4k-htpc-build/)
- [Installing macOS Mojave 10.14 on Gigabyte Z370N WIFI + i7 8700K + UHD 630](https://www.insanelymac.com/forum/topic/335847-guide-gigabyte-ga-z370n-wifi-i7-8700k-uhd-630-mojave-1014/)
- [clover-configurator-how-to-use](https://mackie100projects.altervista.org/clover-configurator-how-to-use/)
- [how-to-make-a-macos-10-14-mojave-flash-drive-installer](https://hackintosher.com/guides/how-to-make-a-macos-10-14-mojave-flash-drive-installer/)
- [fix wifi card](https://www.tonymacx86.com/threads/...card-into-a-ga-z370n-wifi-motherboard.259300/)
- [hackintosh-vanilla-desktop-guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/gathering-kexts)
- [download kexts](https://hackintosher.com/downloads/kexts/)
- [Guide to fresh installing macOS Mojave on a Hackintosh](https://hackintosher.com/guides/guide-to-fresh-installing-macos-mojave-on-a-hackintosh-10-14/)
- [kext-files-macos](https://hackintosher.com/blog/kext-files-macos/)
update:

1. 20190317:  Samsumg pm981(305RMB from Taobao) is not compatible with Macos 10.14.2, will reboot. no easy solution found yet.

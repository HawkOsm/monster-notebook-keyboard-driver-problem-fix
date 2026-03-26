# Monster Keyboard Backlight Fix (Ubuntu 24.04)

If you have a Monster Tulpar (specifically tested on the T6 V2.1) running Ubuntu 24.04 and cannot get your keyboard backlight to turn on, this repository explains exactly why it happens and how to fix it. 

This fix applies to almost all Monster notebooks, as they share the same underlying barebones chassis (manufactured by Tongfang/Clevo) as brands like System76, XMG, and Tuxedo.

## 🛑 The Problem: Why Standard Tools Fail

If you have tried using OpenRGB, AUCC, or the Tuxedo Control Center, you likely hit a brick wall. Here is the technical breakdown of why:

### 1. The Hardware Architecture (Why OpenRGB fails)
Most standard RGB tools scan your system's internal USB bus for a lighting controller chip (like the ITE 8291). If you run `lsusb` in your terminal and don't see an ITE device, your keyboard lighting isn't routed through USB. Instead, Monster hardwires the lighting directly into the motherboard's **Embedded Controller (EC)**. To change the lights, the operating system needs a kernel driver that can speak directly to the EC via WMI/ACPI calls.

### 2. The Software Lockout (Why Tuxedo Drivers fail)
Tuxedo Computers actually wrote the exact Linux kernel module needed to talk to this Embedded Controller. However, their official driver has a strict, hardcoded vendor check. When the driver loads, it reads your motherboard's DMI/BIOS data. When it reads `MONSTER` as the system vendor instead of `TUXEDO`, it intentionally blocks the connection, throwing a `No such device` error and refusing to load.

## ✅ The Solution: The Unlocked Driver

To fix this, we use a community-unlocked version of the Clevo keyboard driver. A Linux hardware vendor named NovaCustom forked the original Tuxedo driver and stripped out the strict brand-name BIOS lockouts. This allows the driver to communicate seamlessly with the Embedded Controller on *any* compatible Tongfang/Clevo chassis, forcing the Monster notebook lights to wake up.

### Installation Steps

Open your terminal and run this single command to download, compile, and install the unlocked driver automatically:

```bash
wget [https://github.com/wessel-novacustom/clevo-keyboard/raw/master/kb.sh](https://github.com/wessel-novacustom/clevo-keyboard/raw/master/kb.sh) && chmod +x kb.sh && sudo ./kb.sh

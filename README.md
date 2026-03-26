# Ubuntu 24.04 Keyboard Backlight Fix for Monster Notebooks

If you just installed Ubuntu on your Monster notebook (specifically tested on my Tulpar T6 V2.1 running Ubuntu 24.04) and your keyboard lights are completely dead, this guide is for you. 

I spent hours trying to get this working. If you have tried OpenRGB, AUCC, or the Tuxedo Control Center and got "No such device" errors, stop what you are doing. This is the actual fix.

## 🛑 Why Standard Apps Don't Work

If you are not a developer and just want your lights on, skip to the **Installation** section. But if you are curious why this is so broken out of the box:

1. **No USB Connection:** Apps like OpenRGB look for a keyboard connected via an internal USB. Monster laptops don't do this; the lights are wired directly into the motherboard.
2. **The Tuxedo Block:** Tuxedo Computers makes a Linux driver for our exact motherboard type. However, their driver checks your system name. When it reads `MONSTER` instead of `TUXEDO`, it intentionally blocks the connection and refuses to turn on the lights.

We are going to use a community-modified driver (thanks to NovaCustom) that removes this name check so our keyboards can finally turn on.

---

## ✅ Installation Guide (Step-by-Step)

Follow these steps exactly. You do not need any coding experience, just copy and paste.

### Step 1: Open Your Terminal
Press `Ctrl` + `Alt` + `T` on your keyboard to open the terminal.

### Step 2: Run the Installer
Copy the exact line of code below, paste it into your terminal, and press `Enter`. It will ask for your computer password. When you type your password, nothing will show up on screen (this is normal in Linux), just type it and press `Enter`.

```bash
wget [https://github.com/wessel-novacustom/clevo-keyboard/raw/master/kb.sh](https://github.com/wessel-novacustom/clevo-keyboard/raw/master/kb.sh) && chmod +x kb.sh && sudo ./kb.sh
```
*Wait for the terminal to finish downloading and installing the driver.*

### Step 3: 🛑 REBOOT YOUR COMPUTER 🛑
**This is the most important step. Your lights will NOT turn on yet.** The computer needs to restart to load the new driver. 

Close everything and restart your laptop. 

### Step 4: You Have Lights!
When your laptop turns back on, your keyboard should light up in solid **White**. 

You can now use your keyboard shortcuts to control the lights (hold the `Fn` key and press the keys on your Numpad):
* `Fn` + `/` : Turn lights On or Off
* `Fn` + `*` : Cycle through colors
* `Fn` + `+` : Turn brightness up
* `Fn` + `-` : Turn brightness down

---

## 🎨 Bonus: Easy Color Changing Script

If you don't want to use the keyboard shortcuts and want a quick way to change your default boot color from the terminal, you can create a simple script.

Run this command to create a file called `color.sh`:
```bash
echo -e '#!/bin/bash\nif [ -z "$1" ]; then\n  echo "Usage: ./color.sh [RED|GREEN|BLUE|YELLOW|MAGENTA|CYAN|WHITE|BLACK]"\n  exit 1\nfi\necho "options tuxedo_keyboard color=${1^^}" | sudo tee /etc/modprobe.d/tuxedo_keyboard.conf\nsudo rmmod tuxedo_keyboard\nsudo modprobe tuxedo_keyboard\necho "Color changed to ${1^^}!"' > color.sh && chmod +x color.sh
```

Now, whenever you want to change your color instantly, just open a terminal and type:
```bash
./color.sh blue
```
*(You can use red, green, blue, yellow, magenta, cyan, white, or black).*

---

## 🚑 Troubleshooting

**"I did everything and restarted, but my lights are still off!"**

Your computer's **Secure Boot** is likely blocking the driver. You need to turn it off.


1. Restart your computer and rapidly press `F2` (or your specific BIOS key, sometimes `Delete`) to enter the BIOS menu.
2. Use your arrow keys to find the **Security** or **Boot** tab.
3. Find **Secure Boot** and change it from `Enabled` to `Disabled`.
4. Save your changes and exit (usually `F10`). 
5. When Ubuntu boots up, your lights will turn on.
```

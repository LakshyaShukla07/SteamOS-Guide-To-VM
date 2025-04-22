# 🖥️ Running SteamOS in VirtualBox

A complete beginner-friendly guide to running **SteamOS** in a **VirtualBox virtual machine** for development and desktop testing.

> ⚠️ **Gaming is not supported** due to lack of GPU acceleration.

---

## 📘 What is SteamOS?

[SteamOS](https://en.wikipedia.org/wiki/SteamOS) is a Linux-based operating system developed by Valve, used in Steam Machines and the Steam Deck. It is open source with some proprietary components.

---

## ⚠️ Disclaimer

This guide is intended for **testing and development purposes only**. You **cannot game** in this virtual machine as there is **no graphics acceleration**. It is best suited for developers and tinkerers who want to test the SteamOS environment.

---

## 🔧 Requirements

- [VirtualBox](https://www.virtualbox.org/)
- [7-Zip](https://www.7-zip.org/download.html)
- Windows PowerShell
- Internet access

---

## 📥 Step 1: Download & Extract SteamOS

1. Visit the SteamOS download page:  
   👉 https://store.steampowered.com/steamos/download/?ver=steamdeck&snr=

2. Accept the license agreement and click **Download SteamOS Deck Image**.

3. Locate the downloaded `.bz2` file in your **Downloads** folder.

4. Extract using 7-Zip:  
   `Right Click > 7-Zip > Extract Here`

5. Rename the extracted file:

```bash
steamdeck-recovery.img
```

---

## 📁 Step 2: Convert IMG to VDI

1. Open PowerShell in the same folder as the `.img` file:
   > Hold **Shift + Right Click** in white space → Select **"Open PowerShell window here"**

2. Run the following commands:

```powershell
# Rename the recovery image (if needed)
mv .\steamdeck-recovery*.img .\steamdeck-recovery.img

# Convert IMG to VDI for VirtualBox
& "$ENV:ProgramFiles\Oracle\VirtualBox\VBoxManage.exe" convertfromraw --format VDI .\steamdeck-recovery.img .\steamdeck-recovery.vdi
```

---

## 📦 Step 3: Create SteamOS Virtual Machine

1. Launch **VirtualBox**, then click **Machine > New**

2. Set these VM options:

- **Name**: SteamOS  
- **Type**: Linux  
- **Version**: Arch (64-bit)  
- **Memory Size**: 4096 MB or more  
- **Hard Disk**: Create new VDI, Dynamically Allocated, 40 GB  

3. Go to **Settings**:
   - **System** > Enable **EFI**
   - **Processor** > Set at least **2 CPUs**
   - **Display** > Set **Video Memory** to 128 MB
   - **Network** > Set **Attached to**: Bridged Adapter
   - **Storage** > Under SATA Controller, click **Add Hard Disk** → Select `steamdeck-recovery.vdi`

4. Click **OK**, then click **Start > Normal** to boot.

---

## 🧰 Step 4: Install SteamOS

1. Wait for the SteamOS desktop to appear.

2. Open **Terminal with Repair Tools** on the desktop.

3. Run:

```bash
# Identify the VirtualBox drive (usually /dev/sda)
sudo fdisk -l
```

4. Edit the install script:

```bash
nano ./tools/repair_device.sh
```

- Change the `DISK=` value to `/dev/sda` or whatever your drive is
- Change the `DISKSUFFIX=` line to **blank** (delete `p`)

- Save: **CTRL+O**, Enter  
- Exit: **CTRL+X**

5. Run the repair command:

```bash
sudo ./tools/repair_device.sh all
```

- Click **Proceed** to destroy data when prompted.
- After completion, click **Cancel** instead of rebooting.

---

## 🔐 Step 5: Configure System in Chroot

### 🔸 Partition A

```bash
sudo steamos-chroot --disk /dev/sda --partset A
steamos-readonly disable
passwd  # Set password
echo -e '[Autologin]\nSession=plasma.desktop' > /etc/sddm.conf.d/zz-steamos-desktopmode.conf
steamos-readonly enable
exit
```

### 🔹 Partition B

```bash
sudo steamos-chroot --disk /dev/sda --partset B
steamos-readonly disable
passwd  # Set password
echo -e '[Autologin]\nSession=plasma.desktop' > /etc/sddm.conf.d/zz-steamos-desktopmode.conf
steamos-readonly enable
exit
```

6. Shutdown the VM:

```bash
sudo shutdown now
```

---

## 🧹 Step 6: Remove Installer Image

1. In VirtualBox, open **Settings** for SteamOS.

2. Go to **Storage**, right-click the `.img` or unused recovery disk and click **Remove Attachment**.

3. Start the VM again normally.

---

## 🧩 Optional: Install VirtualBox Guest Additions

1. Once SteamOS boots, open the terminal and run:

```bash
passwd  # Verify password
steamos-readonly disable
sudo rm -r /etc/pacman.d/gnupg
sudo pacman-key --init
sudo pacman-key --populate archlinux
```

2. Edit the pacman config:

```bash
sudo nano /etc/pacman.conf
```

- Change repo sections like this:
  ```
  [jupiter]       → [jupiter-rel]
  [core]          → [core-rel]
  [extra]         → [extra-rel]
  [community]     → [community-rel]
  [multilib]      → [multilib-rel]
  ```

- Save with **CTRL+O**, Enter, then **CTRL+X**

3. Update the system:

```bash
sudo pacman -Syu
```

4. In VirtualBox, go to:
   **Devices > Insert Guest Additions CD Image**

5. In SteamOS, open **Dolphin File Manager**, click the CD icon, then open a terminal there:

```bash
sudo ./VBoxLinuxAdditions.run
sudo /sbin/rcvboxadd quicksetup all
sudo reboot now
```

---

## 🎉 You're Done!

- You can now use **SteamOS in full desktop mode** inside VirtualBox.
- **Screen resizing and integration** should now work with Guest Additions installed.

---

---

## 📄 License

Copyright © 2025 Ethan Bonser, ChillVibez Studios
Version: 1.0.0
Published: 2025

This guide is provided for educational and informational purposes only. All trademarks, including SteamOS and VirtualBox, are the property of their respective owners.

You are free to share, copy, and adapt this content for non-commercial use, provided that proper credit is given to the original author. Modification for personal or educational use is allowed, but redistribution for profit without explicit permission is prohibited.

If you improve this guide or adapt it for other platforms, contributions are welcomed via GitHub pull requests.

**Author:** Ethan Tyler Bonser
**Organization:** ChillVibez Studios
**License Type:** MIT License
**LinkedIn:** [linkedin.com/in/ethanbonser](https://www.linkedin.com/in/ethanbonser)
**GitHub:** [github.com/ethanbonser](https://github.com/ethanbonser)


---

## ✉️ Feedback

Maintained by [Ethan Bonser](https://github.com/ethanbonser)  
Feel free to submit issues or improvements via Pull Request.
```

---

✅ **How to use it**:
1. Save the full content above into a `README.md` file.
2. Push it to your GitHub repo (e.g., `steamos-vm-guide`).
3. Add screenshots or badges if you'd like — just let me know and I’ll help format them.

Want me to bundle it into a `.zip` for upload or generate a GitHub repo structure for you? Just say the word!

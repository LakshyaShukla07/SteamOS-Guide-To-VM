# 🖥️ Running SteamOS in VirtualBox

A complete beginner-friendly guide to running SteamOS in a VirtualBox VM for testing and development purposes.  
> ⚠️ **Gaming is not supported** due to lack of GPU acceleration.

---

## 📘 What is SteamOS?

SteamOS is a Linux distribution developed by Valve, designed for gaming systems like the Steam Deck. It is open source with proprietary components.

🔗 [More on SteamOS](https://en.wikipedia.org/wiki/SteamOS)

---

## ⚠️ Disclaimer

This VM setup is **not suitable for gaming**. It is intended for developers and testers who want to explore SteamOS in a virtual environment.

---

## 🔧 Requirements

- [VirtualBox](https://www.virtualbox.org/)
- [7-Zip](https://www.7-zip.org/download.html)
- Windows PowerShell (built-in on Windows)
- Internet access

---

## 📥 Step 1: Download & Extract SteamOS

1. Go to the official Valve SteamOS download page:  
   👉 https://store.steampowered.com/steamos/download/?ver=steamdeck&snr=
2. Accept the license and download the **Steam Deck recovery image**.
3. Extract the `.bz2` archive using **7-Zip**.
4. Rename the extracted file to:

```bash
steamdeck-recovery.img

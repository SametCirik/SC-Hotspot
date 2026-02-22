<div align="center">
  <img alt="SC Hotspot Header" src="https://github.com/user-attachments/assets/d5d8806b-b251-4ddd-9306-2c4b9b798af2" width="100%">
</div>

<br>

<p align="center">
  <img src="https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white" alt="Bash">
  <img src="https://img.shields.io/badge/Arch_Linux-1793D1?style=for-the-badge&logo=arch-linux&logoColor=white" alt="Arch Linux">
  <img src="https://img.shields.io/badge/Linux-Supported-FCC624?style=for-the-badge&logo=linux&logoColor=black" alt="Linux">
  <img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="License">
</p>

<p align="center">
  <a href="./README.md">
    <img src="https://cdnjs.cloudflare.com/ajax/libs/flag-icon-css/3.5.0/flags/4x3/gb.svg" alt="English" width="40">
  </a>
  &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="./README_tr.md">
    <img src="https://cdnjs.cloudflare.com/ajax/libs/flag-icon-css/3.5.0/flags/4x3/tr.svg" alt="Türkçe" width="40">
  </a>
</p>

# SC Hotspot - Smart Wi-Fi Bridge & Recovery for Linux

SC Hotspot is an advanced, fully automated Wi-Fi hotspot tool for Linux. It dynamically calculates your active network frequency to prevent hardware conflicts (Single Radio limitations) and features an intelligent **Debug & Recovery Mode** to bypass military/radar (DFS) channel restrictions. All wrapped in a stylish, dual-themed Tux ASCII Terminal User Interface (TUI).

Ideal for Linux users trying to share internet in dorms, corporate buildings, airports, or hotels where network systems use aggressive load balancing or restricted 5 GHz bands.

---

## Why SC Hotspot?

Corporate networks often rely on Load Balancing. This means your device can constantly be switched between 2.4 GHz and 5 GHz bands or forced into restricted DFS (Dynamic Frequency Selection) channels. Trying to open a standard hotspot in these conditions will crash your Wi-Fi card. 

SC Hotspot solves this by:
1. Instantly calculating the exact matching channel before execution.
2. Turning off power-save modes to prevent connection drops.
3. Automatically catching DFS channel errors (like Channel 116) and launching **SC Debug Mode** to force your network manager into a free, civilian frequency.

---

## Requirements

Before using the script, ensure the following core network utilities are installed on your system.

*(Example installation for Arch Linux)*

```bash
sudo pacman -S iw dnsmasq haveged
yay -S linux-wifi-hotspot
```

*Note: The `haveged` package is strongly recommended to prevent the network from freezing with a "Low entropy" error during the WPA2 handshake.*

---

## Installation

1. Clone the repository and navigate to the directory:

   ```bash
   git clone https://github.com/SametCirik/SC-Hotspot.git
   cd SC-Hotspot
   ```

2. Make both the main and debug scripts executable, then move them to your system's binaries path for global access:

   ```bash
   chmod +x sc-hotspot sc-debug
   sudo cp sc-hotspot /usr/local/bin/
   sudo cp sc-debug /usr/local/bin/
   ```

3. Configure your interface (Optional):
The default network interface is set to `wlo1`. If your system uses a different interface (like `wlan0` or `wlp3s0`), edit the `INTERFACE` variable by running:
`sudo nano /usr/local/bin/sc-hotspot` (and do the same for `sc-debug`).

---

## Usage

After global installation, you can launch the interactive TUI from anywhere in your terminal:

```bash
sc-hotspot
```

* **Customization:** You can type your desired Network Name (SSID) and Password (min 8 chars) directly into the Tux interface. If you leave them blank and press "Enter", default values will be applied.
* **Safety:** The script features an automatic trap. When you terminate the hotspot (`Ctrl+C`), it cleans up any background network locks it created, leaving your system perfectly clean for normal use.

---

## The Feature: SC Debug & DFS Recovery Mode

If your Wi-Fi card is connected to a military-restricted DFS channel (e.g., Channel 116) used in airports or big dorms, standard scripts will just crash with a *"Your adapter cannot transmit to channel 116"* error due to hardware NO-IR locks.

SC Hotspot handles this gracefully:
1. It catches the crash immediately.
2. The screen clears, and the **Red Angry Tux (Recovery Mode)** appears.
3. It scans your current corporate network and lists all the available free/civilian BSSIDs (MAC Addresses) around you.
4. You simply pick a number from the list (preferably a 2.4 GHz channel like 1, 6, or 11).
5. The debug script forcefully locks your NetworkManager to that specific free channel, restores the connection, passes your passwords back, and automatically reignites the hotspot via the main script!

---

## How it Works? (Under the Hood)

* **Frequency Math:** It reads physical frequencies via `iw` and uses IEEE 802.11 mathematical formulas to convert them (e.g., `(Frequency - 2407) / 5` for 2.4 GHz).
* **Stability Override:** It runs `iw dev wlo1 set power_save off` globally to prevent micro-drops that ruin the `create_ap` architecture.
* **BSSID & Band Locking:** It manipulates `nmcli` profiles dynamically to bypass DFS restrictions, and auto-removes these locks upon exit.

---

## Contributing & License
This project is open-source and licensed under the **MIT License**.

Under the terms of this license, you are completely free to **fork this repository, modify the code, distribute it, and even use it for commercial purposes**, provided that you include the original copyright notice and permission notice in your copies. The code is entirely in your hands, in line with the free software philosophy.

If you'd like to improve the project, add new features, or fix bugs, feel free to open an **Issue** or submit a **Pull Request**. Contributions are always welcome!

---

## Developer
This project is developed by [Samet Cırık](https://github.com/SametCirik).

---
slug: mbp-nix
date: 2025-11-26
categories:
  - Linux
---

# The Guide to Stabilizing Linux on a MacBook Pro (Mid-2012)

Installing Linux (specifically Kubuntu 22.04) on a **MacBook Pro 9,2 (Mid-2012 / A1278)** breathes new life into this classic hardware. However, out of the box, users often face a frustrating array of issues: the system freezing upon waking from sleep, random reboots when plugging in power, and crashes after leaving the laptop closed overnight.

<!-- more -->

After extensive debugging and testing, we have arrived at a "Gold Standard" configuration that solves these problems. This guide covers the installation of the proprietary Wi-Fi driver, kernel parameter tuning for the SSD and CPU, disabling problematic hardware features, and fixing USB wake-up conflicts.

> "Hardware Context"  
>   This guide is optimized for the **MacBook Pro 9,2 (Ivy Bridge i5)** equipped with a generic SSD (e.g., XrayDisk). If you are using a standard HDD or a different SSD brand, the storage parameters are still safe to use and highly recommended for stability.

---

## Step 1: Wi-Fi Drivers

The open-source `b43` driver often causes instability with the Broadcom BCM4331 chip found in this laptop, specifically regarding power states. For maximum stability and performance, we must switch to the proprietary `wl` driver.

Open your terminal and run the following commands to ensure the open-source driver is removed and the proprietary one is installed:

```bash
# 1. Remove the open-source driver and firmware installer
sudo apt purge firmware-b43-installer b43-fwcutter

# 2. Install the proprietary Broadcom driver
sudo apt update
sudo apt install bcmwl-kernel-source
```

**Reboot your computer** after this step to load the new driver.

-----

## Step 2: The "Magic" GRUB Configuration

This is the most critical step. We need to modify the kernel boot parameters to handle the MacBook's specific hardware quirks, particularly regarding power management, the Thunderbolt controller, and SSD behavior after long sleep.

1. Edit the GRUB configuration file:

```bash
sudo nano /etc/default/grub
```

2. Locate the line starting with `GRUB_CMDLINE_LINUX_DEFAULT`. Replace the *entire line* with the following configuration:

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi_osi=Darwin acpi_backlight=video i915.enable_psr=0 i915.reset=1 init_on_alloc=0 mitigations=off module_blacklist=thunderbolt,firewire_ohci,firewire_core,firewire_sbp2 acpi_sleep=nonvs pci=noaer pcie_aspm=off intel_idle.max_cstate=1 scsi_mod.use_blk_mq=1 ahci.mobile_lpm_policy=1 libata.force=noncq"
```

3. Save the file (`Ctrl+O`, `Enter`) and exit (`Ctrl+X`).

4. **Crucial:** Apply the changes by updating GRUB:

```bash
sudo update-grub
```

### 🧐 Understanding the Parameters

Why so many options? Here is the breakdown of this "Gold Standard" config:

| Parameter                      | Function                                                                                                                       |
|:------------------------------ |:------------------------------------------------------------------------------------------------------------------------------ |
| **`acpi_osi=Darwin`**          | Exposes macOS-compatible ACPI interfaces, improving hardware recognition.                                                      |
| **`acpi_backlight=video`**     | Ensures screen brightness keys work natively with the Intel GPU.                                                               |
| **`i915.enable_psr=0`**        | Disables "Panel Self Refresh" to prevent screen flickering and GPU hangs.                                                      |
| **`module_blacklist=...`**     | Prevents Thunderbolt/FireWire drivers from loading. These ports cause memory allocation errors and crashes upon wake.          |
| **`acpi_sleep=nonvs`**         | Stops Linux from modifying the NVS memory area, preventing conflicts with Apple's BIOS during sleep.                           |
| **`pci=noaer`**                | Disables Advanced Error Reporting to stop the kernel from panicking over harmless bus errors.                                  |
| **`intel_idle.max_cstate=1`**  | **The anti-freeze fix.** Forces the CPU to stay in a light sleep (C1) state, preventing it from desynchronizing with hardware. |
| **`ahci.mobile_lpm_policy=1`** | Prevents the SATA controller from cutting SSD power too aggressively during long sleeps.                                       |
| **`libata.force=noncq`**       | **The "Long Sleep" fix.** Disables Native Command Queuing. Essential for budget SSDs that freeze after waking from deep sleep. |

-----

## Step 3: Reinforcing the Blacklist

While we added the blacklist to GRUB, it is best practice to ensure these modules are also blocked at the system level, ensuring they never load during the initial RAM disk boot process.

1. Create a blacklist configuration file:

```bash
sudo nano /etc/modprobe.d/blacklist-macbook.conf
```

2. Paste the following content:

```conf
blacklist thunderbolt
blacklist firewire_ohci
blacklist firewire_core
blacklist firewire_sbp2
```

3. Save and exit. Then, update the initial RAM disk (initramfs):

```bash
sudo update-initramfs -u
```

-----

## Step 4: Disabling Hibernation

Linux hibernation (suspend-to-disk) is notoriously unstable on Macs. Furthermore, Ubuntu attempts a "Hybrid Sleep" after a few hours, which crashes this machine because the SSD fails to wake up from the hibernation attempt. We will force the system to use **only** RAM for sleeping (Suspend-to-RAM).

1. Create a configuration directory and file:

```bash
sudo mkdir -p /etc/systemd/sleep.conf.d
sudo nano /etc/systemd/sleep.conf.d/no-hibernate.conf
```

2. Paste the following content:

```ini
[Sleep]
AllowHibernation=no
AllowHybridSleep=no
AllowSuspendThenHibernate=no
SuspendState=mem
```

3. Save and exit. Reload systemd configuration:

```bash
sudo systemctl daemon-reload
```

-----

## Step 5: Fixing Wi-Fi Password Loops

Even with the correct driver, the BCM4331 chip often fails the WPA2 security handshake when Power Saving is enabled, causing the system to repeatedly ask for the Wi-Fi password.

1. Create a NetworkManager configuration file:

```bash
sudo nano /etc/NetworkManager/conf.d/99-wifi-powersave-off.conf
```

2. Paste the following content to permanently disable power saving:

```ini
[connection]
wifi.powersave = 2
```

3. Save and exit. (This takes effect on reboot).

-----

## Step 6: Preventing USB Sleep Crashes

The 2012 MacBook has a hardware bug where USB devices (like ethernet adapters or mice) connected during sleep can confuse the power controller (SMC), causing the system to hang with a pulsing light and unresponsive power button.

We must tell the system **not** to wake up via USB to avoid this conflict.

1. Create a systemd service to disable USB wakeup:

```bash
sudo nano /etc/systemd/system/disable-usb-wakeup.service
```

2. Paste the following content:

```ini
[Unit]
Description=Disable USB Wakeup to prevent sleep crashes

[Service]
Type=oneshot
ExecStart=/bin/sh -c "echo EHC1 > /proc/acpi/wakeup; echo EHC2 > /proc/acpi/wakeup; echo XHC1 > /proc/acpi/wakeup"

[Install]
WantedBy=multi-user.target
```

3. Save, exit, and enable the service:

```bash
sudo systemctl enable --now disable-usb-wakeup.service
```

-----

## Step 7: SSD Maintenance and Sensors

To keep your SSD fast and monitor temperatures safely without crashing the Apple SMC controller.

### Enable TRIM

TRIM helps the SSD manage deleted data blocks.

```bash
sudo systemctl enable --now fstrim.timer
```

### Safe Sensor Monitoring

**Do not run `sensors-detect`** on a MacBook. Probing the proprietary Apple SMC bus can freeze the hardware.

Instead, simply install the sensor package and use it directly, as the kernel already includes the correct driver (`applesmc`):

```bash
sudo apt install lm-sensors
```

You can now check temperatures safely by running the command `sensors` in the terminal.

-----

## Conclusion

After applying these settings and rebooting, the MacBook Pro 2012 is transformed. You can close the lid with confidence, leave it overnight, and open it the next day to a responsive system with stable Wi-Fi and no crashes.

**Summary of Fixes:**

1. **Wi-Fi:** Proprietary `wl` driver + Power Save disabled.
2. **Boot/PCI:** GRUB parameters to ignore Thunderbolt and fix memory maps.
3. **Sleep:** C-States limited to C1 + SATA NCQ disabled for SSD stability.
4. **Hardware:** USB wakeup disabled to prevent SMC lockups.

Enjoy your rock-solid Linux MacBook\!

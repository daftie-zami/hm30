# 📄 Device Boot Summary — Compex WPJ342 (OpenWrt)

This document contains detailed information extracted from the serial boot log of a device running OpenWrt on a MIPS-based SoC.

---

## 🖥️ Hardware Overview

| Component       | Description                                |
|----------------|--------------------------------------------|
| **Board Name**  | Compex WPJ342                              |
| **CPU**         | Atheros AR9342 (MIPS 74Kc, revision 3)     |
| **CPU Frequency** | 550 MHz                                 |
| **RAM**         | 64 MB (0x04000000 bytes)                   |
| **Flash Storage** | 16 MB SPI NOR (Winbond w25q128)         |
| **SoC Reference Clock** | 40 MHz                            |
| **AHB Clock**   | 200 MHz                                    |
| **DDR Clock**   | 400 MHz                                    |
| **UART**        | ttyS0 @ 115200 baud                        |

---

## 💾 Flash Layout (MTD Partitions)

| MTD Partition   | Address Range          | Description                        |
|----------------|------------------------|------------------------------------|
| `u-boot`        | 0x00000000 - 0x00030000 | Bootloader                         |
| `firmware`      | 0x00030000 - 0x00ff0000 | Linux Kernel + Root File System   |
| ↳ `kernel`      | 0x00030000 - 0x001d0000 | Linux Kernel                       |
| ↳ `rootfs`      | 0x001d0000 - 0x00ff0000 | Root File System (SquashFS)       |
| ↳ `rootfs_data` | 0x00560000 - 0x00ff0000 | Persistent writable area (overlay)|
| `art`           | 0x00ff0000 - 0x01000000 | Radio calibration / board data     |

---

## 🧠 Software Summary

### Bootloader

- **Type:** U-Boot
- **Status:** Default environment in use (bad CRC warning present)

---

### Operating System

- **Distro:** OpenWrt
- **Kernel Version:** 4.14.149
- **Build Date:** October 26, 2021
- **GCC Version:** 8.3.0
- **Builder Host:** `yang@ubuntu`
- **Compressed Kernel:** LZMA
- **Root Filesystem:** SquashFS (read-only), overlay with writable `rootfs_data`

### Init System

- **Init Style:** OpenWrt init
- **Boot Modules:** Loaded via `/etc/modules-boot.d/`

### Modules Observed on Boot

- `gpio-button-hotplug`
- `ehci-platform` (USB 2.0 support)
- `nls_cp437`, `nls_iso8859-1`, `fat`, `vfat` (Filesystem modules)
- `usbcore`, `usb_common` (USB stack)

---

## 🌐 Networking and I/O

| Feature             | Details                                  |
|---------------------|------------------------------------------|
| **Ethernet**        | Atheros AG71xx (MDIO managed PHY)        |
| **MAC Address**     | `00:03:7F:09:0B:XX`                       |
| **PCIe**            | PCIe Endpoint (EP) mode enabled          |
| **USB**             | EHCI Controller initialized              |
| **GPIOs**           | Active (button hotplug enabled)          |

---

## 🔐 Security & Memory

| Feature                  | Support Status                         |
|--------------------------|----------------------------------------|
| Kernel Memory Protection | Not available on MIPS 74K architecture |
| JFFS2 Partition          | Not in use (read-only squashfs setup)  |

---

## ⚠️ Notices from Boot Log

- **Bad Environment CRC**:

---

> Indicates that U-Boot environment variables are not initialized or corrupted. You can fix this by running `saveenv` from the U-Boot prompt.

- **Rootfs Mounting**:

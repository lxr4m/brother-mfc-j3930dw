# Brother MFC-J3930DW Driver for Arch Linux 🖨️

A native Arch Linux package for the **Brother MFC-J3930DW** printer.
This package repackages the official Brother Linux drivers into a standard `pacman` package.

```
╔══════════════════════════════════════╗
║  Brother MFC-J3930DW                 ║
║  LPR + CUPS driver for Arch Linux    ║
╚══════════════════════════════════════╝
```

## ✨ Features
✅ Downloads official Brother driver packages

✅ Verifies downloaded files with SHA256 checksums

✅ Repackages Brother `.deb` drivers into an Arch package

✅ Installs CUPS PPD and printer filters

✅ No `debtap` required

✅ No manual extraction required

✅ Supports x86_64 systems with required 32-bit libraries

This package only installs the printer driver.
The printer must be added manually through CUPS.

---

# 📦 Build and Install

Clone the repository:
```bash
git clone <repository-url>
cd brother-mfc-j3930dw
```

Build and install:
```bash
makepkg -si
```

Required dependencies:
* `cups`
* `perl`
* `bash`

Additional x86_64 dependencies:
* `lib32-glibc`
* `lib32-gcc-libs`

---

# 🖨️ Adding the Printer in CUPS
Open:
```
http://localhost:631
```

Go to:
```
Administration → Add Printer
```

Choose:
```
Internet Printing Protocol (IPP)
```

For a secure network connection use IPPS:
```
ipps://PRINTER_IP:443/ipp/print
```

Then select:
```
Brother MFC-J3930DW CUPS
```
as the printer model.

---

# 🔐 Recommended Secure Network Configuration
For encrypted printing, enable HTTPS/IPPS in the printer web interface.

Open:
```
https://PRINTER_IP
```

In the Brother Web Based Management interface:
```
Network
 └── Protocol
      ├── Web Server
      ├── SNMP
      ├── IPP
      ├── Net Scan
      ├── mDNS
      └── SNTP
```

Recommended:
```
HTTPS Web Management: Enabled
IPP over HTTPS: Enabled
```

Create or install an SSL certificate:
```
Network
 └── Security
      └── Certificate
```

Then configure:
```
IPP Server
 └── Web (HTTPS)
 └── IPP (HTTPS)
```

After applying changes, restart the printer network service or reboot the printer.

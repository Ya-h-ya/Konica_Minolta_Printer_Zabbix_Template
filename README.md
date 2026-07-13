# Zabbix Konica Minolta Printer Monitoring Template (SNMP)

An up-to-date, production-ready Zabbix template designed to monitor Konica Minolta bizhub printers and other modern network printers adhering to the standard Printer MIB (RFC 3805).

## 🚀 Why I Built This
While setting up monitoring for a fleet of enterprise network printers for work, I found that the existing public Zabbix templates were either heavily outdated, broken on newer Zabbix versions, or missing critical metrics like granular supply tracking and dynamic active alert states. 

To solve this efficiently, I created this template with Claude AI to map standard Printer MIB objects to native Zabbix 7.x structures. This allowed me to rapidly develop, test, and deploy a lightweight, production-ready monitoring solution where none previously existed.

## 📊 Features & Monitored Metrics

- **System Diagnostics:** Tracks system description, uptime, and auto-detects unhandled device reboots.
- **Printer Status:** Monitors execution status (`hrPrinterStatus`) with built-in value mapping (Idle, Printing, Warmup, etc.).
- **Total Page Counter:** Real-time tracking of `prtMarkerLifeCount` for physical usage tracking and maintenance scheduling.
- **Low Supply Discovery:** LLD rule automatically discovers and maps marker supplies (Black/Color Toner, Drum units, Waste Toner Box) using percentage-based thresholds ($<10\%$ Warning, $<3\%$ High Priority).
- **Paper Tray Discovery:** Automatically maps paper input trays (`prtInputTable`) and tracks empty states, accounting for trays without physical level sensors.
- **Active Alert Discovery:** Dynamic alerts for real-time physical conditions (e.g., paper jams, covers open, missing trays) utilizing `prtAlertTable`.

## 📊 Items
- **Supply level: {#SUPPLY_DESC}:** Remaining toner or drum life percentage.
- **Paper level: {#TRAY_DESC}:** Current amount of paper inside tray. <- (Please do some research if your printer supports Paper level monitoring)
- **Paper max capacity: {#TRAY_DESC}:** Total sheet capacity a tray holds.
- **Alert: {#ALERT_DESC}:** Tracks active hardware errors and warnings.

## ⚙️ Requirements & Compatibility

- **Zabbix Version:** Designed for Zabbix 7.4+
- **Protocol:** SNMP v1 / v2c / v3
- **MIB Dependencies:** Standard Host Resources MIB (RFC 2790) & Printer MIB (RFC 3805)

## 🛠️ Installation & Setup

1. **Import the Template:**
   Go to your Zabbix Web Interface -> **Data collection** -> **Templates** -> **Import** and upload `konica_minolta_printer_template.yaml`.

2. **Link to Hosts:**
   Link the imported template to your target network printers.

3. **Configure SNMP Community:**
   By default, the template uses the `{$SNMP_COMMUNITY}` macro set to `public`. If your infrastructure uses a custom community string, simply override the macro on the host or template level:
   - Navigate to the host -> **Macros** -> Inherited and host macros -> Change `{$SNMP_COMMUNITY}` to your secure string.

## 📄 License
This project is open-source and available under the [MIT License](LICENSE).

# ZBT Z8102AX V2 - OpenWrt Router Configuration

## Router Info
- **Model:** ZBT Z8102AX V2
- **Architecture:** ARMv8 (mediatek/filogic)
- **Firmware:** OpenWrt SNAPSHOT r31338-c18476d0c5
- **LuCI:** Master 25.275.49410~a30c8d8

## Modem Info
- **Model:** Quectel RM500U-EA (5G)
- **USB Mode:** CDC-NCM/RNDIS (usb0)
- **AT Port:** ttyUSB2
- **Carrier:** ethio telecom (MCC=636, MNC=01)
- **PLMN:** 63601

## Network Interfaces

### wwan (Primary - Working)
- **Protocol:** DHCP
- **Device:** usb0 (modem built-in NCM)
- **Status:** Connected, IP: 10.x.x.x
- **Role:** Primary internet connection

### wwan2 (ModemManager - Disabled)
- **Protocol:** ModemManager
- **Device:** /sys/devices/platform/soc/11200000.usb/usb2/2-1/2-1.1
- **APN:** Internet
- **Allowed Mode:** 5g|4g|3g|2g
- **Preferred Mode:** none
- **Status:** Disabled (modem in CDC-NCM mode, ModemManager can't manage data)
- **Note:** Would work if modem switched to QMI/MBIM mode via AT command

## DNS Configuration
- **Local:** 127.0.0.1 (dnsmasq + AdGuard Home on port 5353)
- **Forwards:**
  - 127.0.0.1#5353 (AdGuard Home - ad blocking)
  - 1.1.1.1 (Cloudflare)
  - 1.0.0.1 (Cloudflare secondary)
  - 8.8.8.8 (Google)
  - 8.8.4.4 (Google secondary)
  - 9.9.9.9 (Quad9 - security)

## 5G Band Configuration
- **5G SA Bands:** ALL (n1, n2, n3, n5, n8, n28, n41, n77, n78, n79)
- **5G NSA Bands:** ALL (n41, n78, n79)
- **ModemBand Interface:** wwan2
- **ModemBand Port:** /dev/ttyUSB2

## Notes
- The Quectel RM500U-EA is in CDC-NCM mode by default
- In this mode, the modem handles its own network connection via usb0
- ModemManager can control bands/modes via AT commands on ttyUSB2
- But ModemManager CANNOT establish data connections in CDC-NCM mode
- To use ModemManager for full data management, switch modem to QMI mode:
  - AT+QCFG="usbnet",1 (QMI mode)
  - This requires a modem reboot and will temporarily disconnect

## GitHub Repo
Configuration backup for version control.

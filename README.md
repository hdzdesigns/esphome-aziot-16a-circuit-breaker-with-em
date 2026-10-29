# Aziot 16A Smart Circuit Breaker with Energy Monitoring - ESPHome

This repository contains a highly optimized, fully local ESPHome configuration for the **Aziot 16Amp WiFi + BLE Smart Circuit Breaker (Model: AZSCB02)**. 

Powered by the Beken CB2S chipset, this configuration removes the device from the proprietary cloud, enabling instantaneous local control and real-time power monitoring directly within Home Assistant.

## ✨ Features

* **Real-Time Energy Monitoring:** Calibrated integration with the BL0937 sensor to report Voltage (V), Current (A), Power (W), and Energy (kWh).
* **Stealth Mode LED:** Circuit breakers are often placed in distribution boards or bedrooms. This configuration automatically turns off the bright status LED 90 seconds after any state change or boot to prevent light pollution. It will blink to alert you only if it loses Wi-Fi connection.
* **Offline Dynamic Scheduling:** Includes built-in SNTP time-based scheduling. You can set fallback "Turn On" and "Turn Off" times via Home Assistant. Once set, the breaker will execute these schedules independently, even if your Home Assistant server goes offline.
* **Critical Stability Fixes:** Includes vital firmware patches specifically for Beken chips:
  * Disables the default UART logger (`baud_rate: 0`) to prevent hardware pin conflicts that crash the device.
  * Employs PlatformIO build flags (`-DLT_AUTO_DOWNLOAD_REBOOT=0`) to prevent the Beken OS from hijacking pins and causing random reboots.
* **Optional Bluetooth Proxy:** Pre-configured blocks to easily enable ESPHome Bluetooth Proxy forwarding, extending the reach of your smart home's BLE network.

## 📌 Hardware Reference

| Component | Pin / Value | 
| ----- | ----- |
| **Board** | `cb2s` |
| **Main Relay** | `P26` |
| **Physical Button** | `P10` (Inverted) |
| **Status LED** | `P24` (Inverted) |
| **BL0937 CF Pin** | `P8` (Inverted) |
| **BL0937 CF1 Pin** | `P7` (Inverted) |
| **BL0937 SEL Pin** | `P6` (Inverted) |

## 🚀 Installation

1. **Flash the Device:** Because this uses a Beken chip, you will need to flash it initially using a serial connection (via FTDI adapter) using [ltchiptool](https://github.com/libretiny-eu/ltchiptool), or utilize an over-the-air exploit like [tuya-cloudcutter](https://github.com/tuya-cloudcutter/tuya-cloudcutter) if the firmware version permits.
2. Clone or download this repository.
3. Copy `aziot_16a_circuit_breaker.yaml` to your ESPHome configuration directory.
4. Ensure your `secrets.yaml` contains the required network credentials:

```yaml
# Example secrets.yaml
wifi_ssid: "Your_WiFi_SSID"
wifi_password: "Your_WiFi_Password"
ap_password: "Fallback_AP_Password"
api_key: "Your_Generated_API_Key"
ota_password: "Your_OTA_Password"
```

5. Compile and install the firmware via the ESPHome dashboard. Future updates can be done seamlessly over the air (OTA).

## ⚠️ Important Warnings

* **Do not remove the logger `baud_rate: 0` or the `build_flags` in the YAML.** Doing so will cause the Beken chip to panic, crash, or enter a boot loop.
* **Calibration:** The voltage and current multipliers provided in this repository are tuned for standard devices. However, hardware tolerances vary. If your power readings are slightly off, you may need to adjust the `voltage_divider` and `current_resistor` values using a known load (like a 60W incandescent bulb) and a multimeter.
* **High Voltage:** This is a mains-powered device. Always turn off the main power at your breaker panel before installing, wiring, or servicing this module.

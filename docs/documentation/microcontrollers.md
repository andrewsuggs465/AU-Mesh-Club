# Microcontrollers and Modems

## Microcontrollers (MCUs)

### 1. ESP32 / ESP32-S3

| Feature | Detail |
|---|---|
| **Architecture** | Xtensa LX6 dual-core @ 240 MHz (ESP32); LX7 dual-core @ 240 MHz (S3) |
| **RAM** | 520 KB SRAM (ESP32); 512 KB + optional PSRAM (S3) |
| **Flash** | External, typically 4–16 MB |
| **Wi-Fi** | 802.11 b/g/n 2.4 GHz — yes on both |
| **Bluetooth** | BT Classic + BLE 4.2 (ESP32); BLE 5.0 (S3) |
| **GPIO** | 34 pins (ESP32); 45 pins (S3) |
| **ADC/DAC** | 18× ADC, 2× DAC (ESP32); 20× ADC (S3) |
| **USB** | Requires CP2102/CH340 bridge (ESP32); native USB on S3 |
| **Sleep Current** | ~10 µA deep sleep; ~40–80 mA typical active RX |
| **Operating Voltage** | 3.0–3.6 V |
| **Price Range** | Low (~$2–$8 module) |

**Notes:** Most widely used MCU in Meshtastic hardware. Wi-Fi enables MQTT bridging and web UI, but significantly higher power draw than nRF52840 makes it unsuitable for solar or long-term battery nodes. No native USB on classic ESP32.

---

### 2. nRF52840

| Feature | Detail |
|---|---|
| **Architecture** | ARM Cortex-M4F @ 64 MHz |
| **RAM** | 256 KB SRAM |
| **Flash** | 1 MB internal |
| **Wi-Fi** | None |
| **Bluetooth** | BLE 5.0 + Bluetooth Mesh + NFC |
| **GPIO** | 48 pins |
| **ADC** | 8× 12-bit SAR ADC |
| **USB** | Native USB 2.0 Full Speed |
| **Sleep Current** | ~2 µA deep sleep; as low as ~11 µA in typical Meshtastic standby |
| **Operating Voltage** | 1.7–5.5 V |
| **Price Range** | Mid (~$10–$20 module) |

**Notes:** Best-in-class power efficiency — Meshtastic strongly recommends it for solar and handheld applications. Native USB with UF2 drag-and-drop flashing. No Wi-Fi means no web UI without external hardware.

---

### 3. STM32WL / STM32WB (ARM Cortex-M4)

| Feature | Detail |
|---|---|
| **Architecture** | ARM Cortex-M4 @ 48 MHz + Cortex-M0+ radio coprocessor (WL); dual Cortex-M4 (WB) |
| **RAM** | 64–256 KB SRAM |
| **Flash** | 256 KB – 1 MB internal |
| **Wi-Fi** | None |
| **Bluetooth** | BLE 5.0 (STM32WB only) |
| **Sub-GHz Radio** | Integrated SX1262-compatible LoRa/FSK radio (STM32WLE5) |
| **GPIO** | 20–73 pins depending on variant |
| **ADC** | 12-bit multi-channel ADC |
| **USB** | Full Speed on select variants |
| **Sleep Current** | ~300 nA shutdown; ~2 µA standby |
| **Operating Voltage** | 1.71–3.6 V |
| **Price Range** | Mid (~$5–$15 chip) |

**Notes:** STM32WLE5 integrates an SX1262-equivalent LoRa radio on-die, eliminating the external radio chip — smaller PCB, lower BOM cost. Ultra-low shutdown current and hardware AES-256. Steeper toolchain (STM32CubeIDE), limited Meshtastic community support, and UFBGA package is difficult to hand-solder.

---

### 4. ARM + Linux SoC (e.g. Raspberry Pi / CM4)

| Feature | Detail |
|---|---|
| **Architecture** | ARM Cortex-A53/A72 @ 1.5–2.4 GHz |
| **RAM** | 512 MB – 8 GB |
| **Storage** | microSD / eMMC |
| **Wi-Fi** | 802.11ac dual-band (Pi 4/5, CM4) |
| **Bluetooth** | BLE 5.0 |
| **USB** | USB 2.0 + USB 3.0 |
| **GPIO** | 40-pin header |
| **OS** | Full Linux (Debian / Raspberry Pi OS) |
| **Idle Power** | ~600 mA – 2+ A @ 5 V |
| **Boot Time** | 20–60 seconds |

**Notes:** Best treated as a fixed gateway or router node. Runs Meshtastic as a `meshtasticd` daemon alongside MQTT, web servers, and automation scripts. High continuous power draw and boot latency make battery operation and sleep/wake cycles impractical. Requires an external LoRa HAT.

---

## LoRa Modems

> **Generation overview:** Semtech has released multiple generations of LoRa silicon. Gen 1 (SX127x) is legacy. Gen 2 (SX126x) is the current mainstream recommended family. Gen 3 (LR11xx) adds multi-band and geolocation. The STM32WLE5 integrates a Gen 2-equivalent radio directly on the MCU die.

---

### 1. SX1262 / SX1268 — Gen 2 (Recommended)

| Feature | Detail |
|---|---|
| **Frequency Bands** | SX1262: 150–960 MHz (868/915 MHz focus); SX1268: 410–810 MHz (433/470 MHz focus) |
| **Modulation** | LoRa, (G)FSK, BPSK |
| **Max TX Power** | +22 dBm |
| **Receive Sensitivity** | –148 dBm @ SF12, 125 kHz BW |
| **Spreading Factors** | SF5–SF12 |
| **Bandwidth** | 7.8–500 kHz |
| **Max Air Data Rate** | ~62.5 kbps (LoRa mode) |
| **Interface** | SPI |
| **Supply Voltage** | 1.8–3.7 V |
| **RX Current** | ~4.6 mA |
| **TX Current** | ~118 mA @ +22 dBm |
| **Sleep Current** | ~900 nA |
| **Package** | QFN-24 (4×4 mm) |

**Notes:** The dominant modem in current Meshtastic hardware — strongly recommended for all new builds. Roughly half the RX current of SX127x, +2 dBm more TX power, higher data rate, smaller package, and lower supply voltage. SX1262 and SX1268 are pin-compatible and share the same driver. SX1261 is a lower-output sibling (+15 dBm max) used in ultra-low-power designs.

---

### 2. SX1276 / SX1278 — Gen 1 (Legacy)

| Feature | Detail |
|---|---|
| **Frequency Bands** | SX1276: 137–1020 MHz; SX1278: 137–525 MHz |
| **Modulation** | LoRa, (G)FSK, OOK |
| **Max TX Power** | +20 dBm |
| **Receive Sensitivity** | –148 dBm @ SF12, 125 kHz BW |
| **Spreading Factors** | SF6–SF12 |
| **Bandwidth** | 7.8–500 kHz |
| **Max Air Data Rate** | ~37.5 kbps (LoRa mode) |
| **Interface** | SPI |
| **Supply Voltage** | 3.3 V |
| **RX Current** | ~10–12 mA |
| **TX Current** | ~120 mA @ +20 dBm |
| **Sleep Current** | ~200 nA |
| **Package** | QFN-28 (6×6 mm) |

**Notes:** Still found on early T-Beam and first-generation Heltec boards. Functional and inter-operable with SX126x nodes, but Meshtastic actively discourages new builds with these chips — lower efficiency PA, higher RX current, 3.3 V only, and larger package than SX126x.

---

### 3. SX1280 — Gen 2 (2.4 GHz)

| Feature | Detail |
|---|---|
| **Frequency Band** | 2.400–2.500 GHz |
| **Modulation** | LoRa, (G)FSK, FLRC, BLE advertising compatible |
| **Max TX Power** | +12–13 dBm (standard); +20 dBm with external PA (used on T-Beam Supreme) |
| **Receive Sensitivity** | –132 dBm @ SF12, 203 kHz BW (LoRa) |
| **Spreading Factors** | SF5–SF12 |
| **Max Air Data Rate** | 1.6 Mbps (FLRC mode) |
| **Interface** | SPI |
| **Supply Voltage** | 1.8–3.7 V |
| **RX Current** | ~5.4 mA |
| **Sleep Current** | ~900 nA |
| **Package** | QFN-24 |

**Notes:** Operates on the globally license-free 2.4 GHz ISM band — useful for international deployments without frequency planning. Reduced range vs. sub-GHz LoRa (2.4 GHz penetrates walls and foliage less effectively), but well-suited for high-density, short-range mesh. FLRC mode enables very high data rates. Used on the LILYGO T3S3 and T-Beam Supreme with PA.

---

### 4. LR1110 / LR1120 / LR1121 — Gen 3 (Multi-band + Geolocation)

| Feature | Detail |
|---|---|
| **Frequency Bands** | 150–960 MHz sub-GHz + 2.4 GHz ISM; LR1120/1121 also add S-band (1.9–2.1 GHz satellite); LR1121 adds L-band (1.55 GHz) |
| **Modulation** | LoRa, (G)FSK, BPSK, LR-FHSS |
| **Max TX Power** | +22 dBm (sub-GHz HP path); +15 dBm (LP path); +13 dBm (2.4 GHz / S-band) |
| **Receive Sensitivity** | –148 dBm @ SF12, 125 kHz BW — identical to SX1262 |
| **Spreading Factors** | SF5–SF12 |
| **Interface** | SPI (up to 16 MHz) |
| **Supply Voltage** | 1.8–3.7 V |
| **Sleep Current** | ~900 nA |
| **Package** | QFN-40 |
| **On-chip GNSS** | Multi-constellation scanner (GPS, BeiDou) + passive Wi-Fi AP MAC scanner for cloud-assisted geolocation |
| **SX1262 Compatible** | Yes — air interface fully compatible with SX1261/2/8 family |

**Notes:** Supported and growing. The defining feature is the on-chip GNSS and Wi-Fi scanning engine, enabling low-power position fixes without a dedicated GPS module — used in the SenseCAP T1000-E card tracker and Elecrow ThinkNode M3. LR1110 = sub-GHz + 2.4 GHz; LR1120 adds S-band satellite; LR1121 adds L-band on top. **Important:** LR11xx nodes cannot receive packets from legacy SX127x (Gen 1) devices.

---

### 5. STM32WLE5 Integrated Radio — Gen 2 (SoC)

| Feature | Detail |
|---|---|
| **Frequency Bands** | 150–960 MHz |
| **Modulation** | LoRa, (G)FSK, BPSK, OOK |
| **Max TX Power** | +22 dBm (HP path); +15 dBm (LP path) |
| **Receive Sensitivity** | –148 dBm @ SF12, 125 kHz BW |
| **Spreading Factors** | SF5–SF12 |
| **Radio Architecture** | SX1262-compatible IP licensed from Semtech, running on dedicated Cortex-M0+ radio processor |
| **Sleep Current** | ~300 nA shutdown (entire SoC including MCU) |
| **Package** | UFBGA73 or QFN48 |
| **SX1262 Compatible** | Yes |

**Notes:** SX1262-equivalent RF performance in a single-chip SoC — eliminates the external radio, SPI bus, and RF matching network entirely. Best suited for compact custom hardware. Limited Meshtastic community support; main barrier is STM32 toolchain complexity.

---

## Summary Tables

### MCUs

| | ESP32 | ESP32-S3 | nRF52840 | STM32WL/WB | ARM+Linux |
|---|---|---|---|---|---|
| **Wi-Fi** | ✅ | ✅ | ❌ | ❌ | ✅ |
| **BLE** | 4.2 | 5.0 | 5.0 | 5.0 (WB) | 5.0 |
| **Native USB** | ❌ | ✅ | ✅ | Varies | ✅ |
| **Battery Life** | Medium | Medium | Best | Excellent | Worst |
| **Meshtastic Support** | Best | Best | Good | Limited | Good (daemon) |
| **Cost** | Low | Low | Mid | Mid | High |
| **Best Use Case** | MQTT gateway, indoor | Gateway, touchscreen UI | Solar, handheld | Custom hardware | Fixed router |

### LoRa Modems

| | SX1262/68 | SX1276/78 | SX1280 | LR1110–1121 | STM32WLE5 |
|---|---|---|---|---|---|
| **Generation** | Gen 2 | Gen 1 | Gen 2 | Gen 3 | Gen 2 (SoC) |
| **Frequency** | Sub-GHz | Sub-GHz | 2.4 GHz | Sub-GHz + 2.4 GHz (+ satellite) | Sub-GHz |
| **Sensitivity** | –148 dBm | –148 dBm | –132 dBm | –148 dBm | –148 dBm |
| **Max TX** | +22 dBm | +20 dBm | +13 dBm (+20 w/PA) | +22 dBm | +22 dBm |
| **RX Current** | ~4.6 mA | ~12 mA | ~5.4 mA | ~4.6 mA | ~4.6 mA |
| **On-chip GNSS** | ❌ | ❌ | ❌ | ✅ | ❌ |
| **External chip needed** | Yes | Yes | Yes | Yes | No (on-die) |
| **SX127x compatible** | ✅ | — | ❌ | ❌ | ✅ |
| **Meshtastic status** | **Recommended** | Legacy | Niche | Growing | Limited |

#### Pre-Built Nodes

If you prefer a simpler option, consider pre-built devices such as the **Heltec V3/V4**, **WisMesh Pocket V2 / Tag**, or **T-Echo**, many of which include a case.

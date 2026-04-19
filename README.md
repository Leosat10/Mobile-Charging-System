# 🪙 Smart Coin-Based Mobile Charging System

> A coin-operated mobile charging station built with ATmega328, a multi-coin sensor, relay modules, and a 20×4 I²C LCD. Insert a coin → get 2 minutes of charging. Stack coins for more time. Designed for public spaces.

---

## 🌐 Live Demo (Web Showcase)

👉 **[View the interactive project page → leosat10.github.io](https://leosat10.github.io)**

Experience a live simulation of the coin insertion, port selection, and countdown timer — no hardware needed!

---

## 📰 Full Project Article on Hackster.io

👉 **[Read the full build guide on Hackster.io](https://www.hackster.io/vsatish2k4/mobile-charging-system-d4f938)**

Includes complete circuit schematics, wiring diagrams, component list, and full Arduino source code.

---

## 💡 How It Works

1. **Insert a coin** → Coin sensor validates it and signals the microcontroller
2. **Credits stack** → Each coin adds 2 minutes of charging time
3. **Select a port** → Press one of 4 buttons to assign credit to a charging station
4. **Relay activates** → 5V USB power is switched ON for that port
5. **Countdown runs** → LCD displays remaining time in real-time
6. **Auto cutoff** → When time reaches 00:00, relay opens and charging stops
7. **Top-up anytime** → Insert more coins during a session to add time

---

## 🧰 Hardware Components

| Component | Details |
|---|---|
| ATmega328 Microcontroller | Core MCU · 32KB flash · I²C + UART support |
| Multi Coin Sensor | Validates coin type · outputs pulse per valid coin |
| Relay Module × 4 | Controls 5V USB power per charging port |
| LCD 20×4 I²C | Displays coin count + countdown per port |
| Low Voltage Transformer Class II | Isolated safe power supply for public use |
| Buzzer | Audible beep on coin insertion confirmation |
| Push Buttons × 4 | Port selection by user |

---

## 💻 Arduino Firmware (Quick Look)

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 20, 4);

const unsigned long chargeTimePerCoin = 120; // 2 minutes per coin

void loop() {
  // Coin detection
  if (digitalRead(coinSlotPin) == LOW) {
    coinsInserted++;
    tone(buzzerPin, 1000, 100); // beep on insert
  }

  // Button press → start charging on selected port
  for (int i = 0; i < 4; i++) {
    if (digitalRead(buttonPins[i]) == LOW && coinsInserted > 0) {
      startCharging(i); // activate relay + set timer
    }
  }

  // Auto cutoff when timer expires
  for (int i = 0; i < 4; i++) {
    if (relayEndTimes[i] > 0 && millis() >= relayEndTimes[i])
      stopCharging(i);
  }

  updateLCD(); // refresh countdown display
}
```

Full source code available on **[Hackster.io](https://www.hackster.io/vsatish2k4/mobile-charging-system-d4f938)**

---

## 📊 System Specs

| Parameter | Value |
|---|---|
| Charge time per coin | 2 minutes (120 seconds) |
| Number of ports | 4 simultaneous stations |
| Output voltage | 5V USB |
| Display | 20×4 I²C LCD |
| Coin top-up | Supported during active session |
| Auto cutoff | Yes — relay opens at 00:00 |

---

## 🚀 Possible Upgrades

- GSM module for remote monitoring
- Digital payment (UPI / QR code) support
- IoT dashboard for usage analytics
- Weatherproof enclosure for outdoor use
- Solar panel power input

---

## 👤 Author

**LEOSAT** — [@vsatish2k4 on Hackster.io](https://www.hackster.io/vsatish2k4)

---

## 📄 License

Open Source Hardware · MIT License · © 2025

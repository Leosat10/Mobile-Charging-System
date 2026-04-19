# Smart Coin-Based Mobile Charging System

**Public‑friendly, coin‑operated USB charging station** – insert a coin, get 2 minutes of power. Stack coins for more time, choose your port, and watch the countdown. Designed for malls, cafes, transit hubs, and waiting areas.

---

## Live Demo & Full Article

| Experience the simulation | Read the build guide |
| :---: | :---: |
| [**Live Web Showcase**](https://leosat10.github.io) <br> *Insert coins, select a port, watch the timer – no hardware needed* | [**Full Project Article on Hackster.io**](https://www.hackster.io/) <br> *Circuit schematics, wiring, code, and component list* |

---

## How It Works – Step by Step

1. **Insert a coin** – Coin sensor validates it and signals the microcontroller.  
2. **Credits stack** – Each valid coin adds 2 minutes of charging time.  
3. **Select a port** – Press one of 4 buttons to assign credit to a charging station.  
4. **Relay activates** – 5V USB power is switched ON for that port.  
5. **Countdown runs** – LCD displays remaining time in real‑time.  
6. **Auto cutoff** – When time reaches 00:00, relay opens and charging stops.  
7. **Top‑up anytime** – Insert more coins during an active session to add time.

---

## Hardware Components

| Component | Details |
| :--- | :--- |
| **ATmega328 Microcontroller** | Core MCU · 32KB flash · I²C + UART support |
| **Multi Coin Sensor** | Validates coin type · outputs pulse per valid coin |
| **Relay Module (x4)** | Controls 5V USB power per charging port |
| **LCD 20×4 I²C** | Displays coin count + countdown per port |
| **Low Voltage Transformer** | Class II isolated safe power supply for public use |
| **Buzzer** | Audible beep on coin insertion confirmation |
| **Push Buttons (x4)** | Port selection by user |

---

## Arduino Firmware (Quick Look)

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 20, 4);

const unsigned long chargeTimePerCoin = 120; // 2 minutes per coin

void loop() {
  // Coin detection
  if (digitalRead(coinSlotPin) == LOW) {
    coinsInserted++;
    tone(buzzerPin, 1000, 100);  // beep on insert
  }

  // Button press → start charging on selected port
  for (int i = 0; i < 4; i++) {
    if (digitalRead(buttonPins[i]) == LOW && coinsInserted > 0) {
      startCharging(i);   // activate relay + set timer
    }
  }

  // Auto cutoff when timer expires
  for (int i = 0; i < 4; i++) {
    if (relayEndTimes[i] > 0 && millis() >= relayEndTimes[i])
      stopCharging(i);
  }

  updateLCD();  // refresh countdown display
}

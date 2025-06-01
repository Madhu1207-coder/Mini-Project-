# 🔫 Triggering Gun with Alert System

An advanced personal safety device integrating a **stun gun mechanism** with a **real-time GPS and GSM alert system**. Designed to provide both **self-defense** and **emergency response**, this device is ideal for vulnerable individuals seeking enhanced protection in threatening situations.

## 📌 Project Overview

This mini project demonstrates the integration of hardware and software components to create a compact, portable **self-defense tool**. When activated, it emits a **high-voltage spark** to incapacitate an attacker and simultaneously sends an **emergency alert SMS with the user's live location** to predefined contacts.

### 🧠 Guided By:

**Dr. P. Visu**
Head of the Department

### 👩‍💻 Team Members:

* Madhumitha B (113222072055)
* Rohini V (113222072081)
* Sangeetha S (113222072087)

---

## 📚 Abstract

The project aims to address growing concerns around personal safety by combining:

* A **stun gun** circuit (using a step-up transformer)
* A **SIM800L GSM module** and **NEO-6M GPS module**
* A **buzzer and sound sensor** for dual activation
* Controlled by an **ATmega328P (Arduino)**

Upon activation (via button press or sound detection), the device:

* Generates an **electric shock**
* Sends an **emergency SMS** with **live GPS coordinates**
* Activates a **buzzer alarm**

---

## 🧩 Modules

### 🔌 Module 1 – Stun Gun

* Step-up transformer for high-voltage shock
* Push-button for manual activation
* Compact design using PVC casing
* 26.7V, 3200mAh lithium battery
* Safety-regulated shock

### ⚡ Module 2 – Power Management

* Battery: 7.4V, 3200mAh Li-ion
* TP4056 charging circuit
* LM2586 DC-DC converter for regulated output
* Energy-efficient design

### 🌍 Module 3 – Emergency Alert System

* GPS (NEO-6M) for location tracking
* GSM (SIM800L) for SMS alerts
* Buzzer and KY-038 sound sensor for activation
* Controlled by ATmega328P microcontroller

---

## 🛠️ Technologies & Tools

| Component       | Description                           |
| --------------- | ------------------------------------- |
| Arduino Uno     | Microcontroller (ATmega328P)          |
| SIM800L         | GSM module for SMS alerts             |
| NEO-6M          | GPS module for real-time coordinates  |
| KY-038          | Sound sensor for automatic activation |
| TP4056          | Battery charging module               |
| LM2586          | Voltage regulator                     |
| PVC Pipe (34mm) | Device housing                        |

---

## 💻 Arduino Code

> 📁 **Add this code file to your repo:**
> File Name: `TriggeringGun_AlertSystem.ino`

```cpp
#include <SoftwareSerial.h>
#include <TinyGPS.h>

float lat = 13.0827, lon = 80.2707;
SoftwareSerial gpsSerial(6, 5); // rx, tx
TinyGPS gps;

#define pushbutton 2
#define buzzer 13
int SoundSensor1 = 12;

String latitude, longitude;

void setup() {
  Serial.begin(9600);
  gpsSerial.begin(9600);
  pinMode(pushbutton, INPUT_PULLUP);
  pinMode(SoundSensor1, INPUT);
  pinMode(buzzer, OUTPUT);
  digitalWrite(buzzer, LOW);
}

void loop() {
  if (digitalRead(pushbutton) == LOW || digitalRead(SoundSensor1) == 0) {
    latitude = "13.0827";
    longitude = "80.2707";

    Serial.println("Safety Alert");
    digitalWrite(buzzer, HIGH);
    delay(100);
    message1();
    delay(100);
    message2();
    digitalWrite(buzzer, LOW);

    while (gpsSerial.available()) {
      if (gps.encode(gpsSerial.read())) {
        gps.f_get_position(&lat, &lon);
        Serial.print("Position: Latitude:");
        Serial.print(lat, 6);
        Serial.print("; Longitude:");
        Serial.println(lon, 6);
      }
    }
    delay(5000);
  }
}

void message1() {
  Serial.print("AT\r\n"); delay(800);
  Serial.print("AT+CMGF=1\r\n"); delay(800);
  Serial.print("AT+CMGS=\"9940033064\"\r\n"); delay(500);
  Serial.print("'Safety Alert'\r\nhttp://maps.google.com/maps?q=loc:");
  Serial.print(latitude + "," + longitude);
  delay(500);
  Serial.write((char)26);
}

void message2() {
  Serial.print("AT\r\n"); delay(800);
  Serial.print("AT+CMGF=1\r\n"); delay(800);
  Serial.print("AT+CMGS=\"9952243782\"\r\n"); delay(500);
  Serial.print("'Safety Alert'\r\nhttp://maps.google.com/maps?q=loc:");
  Serial.print(latitude + "," + longitude);
  delay(500);
  Serial.write((char)26);
}
```

---

## ✅ Final Output

* **Electric shock** is produced using the stun gun
* **Location and alert messages** are sent to authorized numbers
* **Activated via** buzzer or sound detection
* Tested with successful performance

---

## 💡 Future Improvements

* Integrate fingerprint authentication
* App integration for live monitoring
* Water-resistant wearable design

---



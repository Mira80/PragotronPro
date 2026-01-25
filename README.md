# 🕰️ Pragotron Pro - Firmware v7.8.0

**Platforma:** Wemos D1 Mini (ESP8266) + L298N Driver + OLED Shield
**Aktuální verze:** 7.8.0

Vítejte v oficiálním repozitáři pro **Pragotron Master Control**. Tento projekt promění čip ESP8266 v profesionální řídící jednotku pro podružné hodiny (systém Pragotron/Elektročas) s minutovými nebo sekundovými pulzy.

> **ℹ️ Poznámka:** Tento repozitář slouží k distribuci zkompilovaného firmwaru pro snadnou instalaci přes prohlížeč.

---

## 🚀 Rychlá Instalace (Web Installer)

K nahrání softwaru nepotřebujete Arduino IDE, ovladače ani stahovat binární soubory. Stačí prohlížeč (Chrome/Edge) a USB kabel.

1.  Připojte Wemos D1 Mini k počítači přes USB.
2.  Klikněte na tlačítko níže (otevře instalátor).
3.  Vyberte **"CONNECT"** a zvolte příslušný COM port.
4.  Klikněte na **"INSTALL PRAGOTRON"**.

👉 **[SPUSTIT WEB INSTALLER](https://mira80.github.io/PragotronPro/)**

---

## ✨ Novinky ve verzi v7.8.0 (Stable)

* **🛡️ Robustní řízení minut (Zero-Loss Logic):** Zcela přepsaná logika generování impulzů. Systém již nečeká na "nultou sekundu", ale aktivně sleduje změnu času. Tím je zaručeno, že hodiny **nevynechají minutu** ani při krátkodobém zaseknutí WiFi nebo synchronizaci NTP.
* **📅 Datum sestavení (Build Time):** V menu *Aktualizace* a v patičce *Nápovědy* se nyní zobrazuje přesné datum a čas kompilace firmwaru. Máte tak jistotu, že běžíte na nejnovější verzi.
* **🎨 Retro Design:** Stabilizované grafické rozhraní ve stylu překlápěcích hodin (Split-flap).
* **Captive Portal:** Automatické přihlášení do konfigurace po připojení k `Pragotron_AP`.
* **Smart Features:** Zachovány funkce jako *Smart Kalibrace*, *Stop Timer* (časovač zastavení) a *Dynamický titulek* webu podle umístění hodin.

---

## 🔌 Zapojení Hardware (Wemos D1 Mini + L298N)

Pro řízení 24V (nebo 12V) linky hodin používáme modul **L298N**. Wemos D1 Mini posílá logické signály (3.3V), které L298N zesílí na potřebné napětí pro cívky.

### 1. Tabulka propojení

| Linka | Pin Wemos | GPIO | Vstup L298N | Výstup L298N (Cívka) |
| :--- | :--- | :--- | :--- | :--- |
| **Minuty A** | D6 | 12 | **IN1** | **OUT1** |
| **Minuty B** | D5 | 14 | **IN2** | **OUT2** |
| **Sekundy A**| D0 | 16 | **IN3** | **OUT3** |
| **Sekundy B**| D7 | 13 | **IN4** | **OUT4** |

### 2. Důležité poznámky k L298N
* **Napájení cívek:** Do svorky `12V` na L298N přiveďte napětí zdroje pro hodiny (např. 24V DC).
* **Společná zem (GND):** Je kritické spojit `GND` Wemosu, `GND` modulu L298N a `GND` zdroje 24V!
* **Enable Jumpery:** Nechte nasazené propojky na pinech `ENA` a `ENB` na modulu L298N.
* **OLED Displej:** I2C Shield se nasadí přímo na Wemos (piny D1/D2).

---

## ⚡ Varianty Zapojení (Schémata)

Vyberte si variantu podle toho, zda potřebujete zálohu při výpadku proudu (UPS).

### VARIANTA A: Rozšířené zapojení (s UPS Detekcí)
*Použijte, pokud chcete, aby hodiny po výpadku proudu automaticky "dohnaly" čas.*
* **Nastavení:** V aplikaci povolte: `Povolit UPS (A0)`.

```text
                                [ NAPÁJENÍ A UPS DETEKCE ]

   Vstup 5V (USB/Zdroj)
         |
         +-----------+--------------------------------------------------+
         |           |                                                  |
         |          [R1] (Rezistor 10k-22k)                             |
         |           |                                                  |
         |           +----------------------> PIN A0 (Wemos)            |
         |           |                        (Měří napětí PŘED diodou) |
         |          [R2] (Rezistor 10k-22k)                             |
         |           |                                                  |
         |          GND                                                 |
         |                                                              |
         +-------->| D |----------+-----------+------------------------>+---> Wemos 5V
                  (Dioda)         |           |                         |
                                  |           |                         +---> L298N Logic 5V
                                ===== (+)     |
                          (C1)  =====         |
                        2200uF    |           |
                                  |           |
         GND ---------------------+-----------+------------------------>+---> Wemos GND
                                                                        |
                                                                        +---> L298N GND

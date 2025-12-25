# 🕰️ Pragotron Master Control - Web Installer

**Aktuální verze:** 6.8 "Final Safety"
**Platforma:** Wemos D1 Mini (ESP8266)

Vítejte v oficiálním repozitáři pro firmware **Pragotron Master Control**. Tento projekt slouží k instalaci řídícího softwaru pro podružné hodiny (systém Pragotron/Elektročas) do čipu ESP8266.

> **ℹ️ Poznámka:** Tento repozitář slouží výhradně pro distribuci zkompilovaného a otestovaného firmwaru. Zdrojový kód není veřejný, aby byla zajištěna 100% kompatibilita a stabilita pro koncové uživatele bez nutnosti složité kompilace.

## 🚀 Instalace (Web Installer)

Pro nahrání firmwaru nepotřebujete žádný software ani programátorské znalosti. Stačí vám prohlížeč (Chrome, Edge) a USB kabel.

1.  Připojte Wemos D1 Mini k počítači přes USB.
2.  Klikněte na tlačítko níže (nebo otevřete stránku instalátoru):
3.  Vyberte "CONNECT" a zvolte příslušný COM port.
4.  Klikněte na "INSTALL PRAGOTRON".

👉 **[SPUSTIT WEB INSTALLER](https://vase-uzivatelske-jmeno.github.io/nazev-repozitare/)**
*(Zde prosím doplňte odkaz na vaši GitHub Pages stránku s instalátorem)*

---

## ✨ Klíčové Funkce Verze 6.8

* **Total Recall (UPS):** Inteligentní dopočítání zameškaného času po výpadku proudu (pamatuje si přesný čas smrti i frontu impulzů).
* **Safety Switch:** Možnost v nastavení zapnout/vypnout sledování napětí na pinu A0 (ochrana proti náhodným restartům u desek bez HW úpravy).
* **Webová konfigurace:** Kompletní nastavení WiFi, NTP a parametrů hodin přes mobil/PC.
* **Ochrana polarity:** "Atomic Pairs" logika zajišťuje, že se cívky hodin nikdy nepřepólují.
* **Diagnostika:** OLED displej ukazuje stav synchronizace, letního času a frontu impulzů.

## 🔌 Zapojení Hardware (Wemos D1 Mini)

| Funkce | Pin Wemos | Pin GPIO | Poznámka |
| :--- | :--- | :--- | :--- |
| **Minuty (Lichá)** | D6 | GPIO 12 | Výstup na H-Můstek |
| **Minuty (Sudá)** | D5 | GPIO 14 | Výstup na H-Můstek |
| **Sekundy (Lichá)**| D0 | GPIO 16 | Výstup na H-Můstek |
| **Sekundy (Sudá)** | D7 | GPIO 13 | Výstup na H-Můstek |
| **Sledování UPS** | A0 | ADC 0 | *Volitelné (vyžaduje dělič napětí)* |
| **Displej** | D1/D2 | 5/4 | I2C (SCL/SDA) |

## ⚙️ První Spuštění

1.  Po nahrání firmwaru se hodiny restartují.
2.  Na mobilu vyhledejte WiFi síť **`Pragotron_AP`**.
3.  Připojte se. Měla by se automaticky otevřít konfigurační stránka (nebo jděte na `192.168.4.1`).
4.  Nastavte vaši domácí WiFi, NTP server a uložte.
5.  Hodiny se restartují a připojí k vaší síti.

## ⚠️ Důležité upozornění

Funkce **Power Monitor (A0)** je po instalaci **vypnutá**. 
Pokud máte na desce připojený obvod pro detekci výpadku napětí (dělič na pinu A0), musíte tuto funkci ručně povolit v *Nastavení -> Povolit UPS / Power Monitor*. Bez HW úpravy tuto funkci nezapínejte!

---
*Pragotron Master Control © 2025*

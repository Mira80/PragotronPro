# 🕰️ Pragotron Master Control - Firmware v7.4.0

**Platforma:** Wemos D1 Mini (ESP8266) + L298N Driver + OLED Shield
**Aktuální verze:** 7.4.0 "Vector Edition"

Vítejte v oficiálním repozitáři pro **Pragotron Master Control**. Tento projekt promění čip ESP8266 v profesionální řídící jednotku pro podružné hodiny (systém Pragotron/Elektročas) s minutovými nebo sekundovými pulzy.

> **ℹ️ Poznámka:** Tento repozitář slouží k distribuci zkompilovaného firmwaru pro snadnou instalaci přes prohlížeč.

---

## 🚀 Rychlá Instalace (Web Installer)

K nahrání softwaru nepotřebujete Arduino IDE, ovladače ani stahovat binární soubory. Stačí prohlížeč (Chrome/Edge) a USB kabel.

1.  Připojte Wemos D1 Mini k počítači přes USB.
2.  Klikněte na tlačítko níže (otevře instalátor).
3.  Vyberte **"CONNECT"** a zvolte příslušný COM port.
4.  Klikněte na **"INSTALL PRAGOTRON"**.

👉 **[SPUSTIT WEB INSTALLER](https://mira80.github.io/PragotronWebInstaller/)**

---

## ✨ Co přináší verze 7.4.0

* **Vektorová Ikona (SVG):** Aplikace má nyní unikátní ikonu čtvercových hodin Pragotron, která je vykreslena přímo kódem (vektorově). Je ostrá na každém zařízení a šetří paměť čipu.
* **OLED Smart Saver:** Opravena logika šetřiče. Běžný minutový pulz hodin již nerozsvěcí displej – hodiny mohou tikat "potmě". Displej se zapne jen na vyžádání nebo při restartu.
* **Live AJAX UI:** Tlačítka v nastavení reagují okamžitě (změna barvy/stavu) bez nutnosti zdlouhavého obnovování celé stránky.
* **Smart Sync:** Inteligentní kalibrace, která umí hodiny nejen dohnat (zrychlené pulzy), ale i pozastavit (čekání na reálný čas).

---

## 🔌 Schéma Zapojení (L298N Dual H-Bridge)

Pro řízení 24V (nebo 12V) linky hodin používáme modul **L298N**. Wemos D1 Mini pouze posílá logické signály (3.3V), které L298N zesílí na potřebné napětí pro cívky.

### 1. Propojení Wemos D1 Mini -> L298N

| Linka | Pin Wemos | GPIO | Vstup L298N | Výstup L298N (Cívka) |
| :--- | :--- | :--- | :--- | :--- |
| **Minuty A** | D6 | 12 | **IN1** | **OUT1** |
| **Minuty B** | D5 | 14 | **IN2** | **OUT2** |
| **Sekundy A**| D0 | 16 | **IN3** | **OUT3** |
| **Sekundy B**| D7 | 13 | **IN4** | **OUT4** |

**Důležité pro L298N:**
* **Napájení:** Do svorky `12V` na L298N přiveďte napětí zdroje pro hodiny (např. 24V DC).
* **Zem (GND):** Spojte `GND` Wemosu s `GND` modulu L298N a `GND` zdroje 24V!
* **Enable Jumpery:** Nechte nasazené propojky na pinech `ENA` a `ENB` na modulu L298N.

### 2. Ostatní Hardware
* **OLED Displej (0.66"):** Nasadí se přímo na Wemos (I2C piny D1/D2).
* **UPS Detekce (Volitelné):** Odporový dělič na pinu `A0` pro detekci napětí zdroje. *(V nastavení nezapínejte, pokud nemáte zapojeno!)*

---

## ⚙️ První spuštění

1.  Po nahrání se hodiny restartují. Na displeji se zobrazí logo.
2.  Na mobilu nebo PC vyhledejte WiFi síť **`Pragotron_AP`**.
3.  Připojte se. Měla by se automaticky otevřít konfigurační stránka (pokud ne, jděte na `192.168.4.1`).
4.  **Nastavte WiFi:** Zadejte název (SSID) a heslo vaší domácí sítě.
5.  **Hardware:** Zkontrolujte délku pulzu (pro běžné minuty cca **1200 ms**).
6.  Uložte. Hodiny se restartují a připojí k vaší WiFi. IP adresa se vypíše na OLED displeji.

---

## 📖 Uživatelský manuál funkcí

### 🏠 Dashboard (Hlavní stránka)
Zobrazuje aktuální stav systému.
* **Digitální hodiny:** Čas synchronizovaný přes internet (NTP).
* **Indikátor FRONT (Queue):** Číslo udává, kolik minutových pulzů "čeká" ve frontě na odvysílání.
* **Indikátor STOP:** Svítí červeně, pokud jsou hodiny zastaveny (manuálně nebo čekají na čas při kalibraci).
* **Indikátor NTP:** Zelená = čas je synchronizován. Červená = chyba sítě.

### 🎮 Ovládání (Menu)
* **Ruční posun:** Jednoduché přidání minut do fronty. Zadejte "5", klikněte a hodiny 5x cvaknou.
* **Smart Kalibrace (Doporučeno):** Slouží k srovnání fyzických hodin s časem na webu.
    1.  Podívejte se na hodiny na zdi (např. ukazují `12:15`).
    2.  Do formuláře zadejte `12` a `15`.
    3.  Klikněte na **Srovnat hodiny**.
    * *Logika:* Pokud jsou hodiny pozadu, systém rychle docvaká rozdíl. Pokud jsou hodiny napřed (např. reálně je teprve 12:10), systém hodiny **zastaví** a počká 5 minut, dokud se časy nesrovnají.
* **Manuální STOP:** Okamžitá brzda. Vhodné pro servis nebo utišení hodin.

### ⚙️ Nastavení (Settings)
* **OLED Displej / Šetřič:**
    * Nastavte čas v minutách pro vypnutí displeje (prevence vypalování).
    * Hodnota `0` = displej svítí trvale.
    * Běžný chod hodin displej neprobouzí. Probudíte ho návštěvou webu nebo tlačítkem v menu.
* **Délka pulzu (ms):**
    * Minutové hodiny (PPH): Doporučeno **1000 - 1500 ms**.
    * Sekundové linky: Doporučeno **200 - 400 ms**.
    * ⚠️ *Varování: Extrémně dlouhé pulzy mohou přehřát cívku hodin.*
* **Povolit UPS (A0):** Funkce pro zálohování polohy při výpadku proudu. Zapínejte **POUZE** pokud máte na pinu A0 připojený detektor napětí (dělič). Pokud funkci zapnete bez hardware, hodiny si budou myslet, že došlo k výpadku a zastaví se ("POWER FAIL").

### 🛡 Bezpečnostní funkce (Watchdog)
Firmware obsahuje ochranu cívek. Pokud by procesor zamrzl nebo nastala chyba, která by nechala cívku sepnutou déle než **5 sekund**, bezpečnostní pojistka ji automaticky odpojí, aby nedošlo ke spálení vinutí hodin.

---
*Pragotron Master Control © 2025 Miroslav Urban*

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

## ✨ Co přináší verze 7.5.0

* **Elegantní Restart:** Už žádné chyby "Web není dostupný" po uložení nastavení. Systém nyní zobrazí vizuální odpočet (20 sekund) a poté automaticky obnoví stránku.
* **Chytré formuláře:** Pole v nastavení (WiFi, Hostname, Čas) se nyní automaticky předvyplňují aktuálními hodnotami. Už nemusíte znovu zadávat heslo k WiFi při změně jiné položky.
* **Vylepšená Identifikace:** Název umístění (např. "Obývák" nebo "Dílna") se nyní zobrazuje v záhlaví všech stránek aplikace pro lepší orientaci.
* **Bezpečnější UX:** Heslo k WiFi je nyní v prohlížeči skryté (hvězdičky). Tlačítka jasně indikují, že akce vyvolá restart ("ULOŽIT A RESTARTOVAT").

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


## 🔌 Detailní Schéma Zapojení
Pro správnou funkci detekce výpadku proudu (UPS/Power Monitor) a bezpečné řízení 24V linky je nutné dodržet toto zapojení.

### Legenda k součástkám:
* **D (Dioda):** Ideálně Schottky (např. 1N5817) pro menší úbytek napětí. Odděluje "živé" napětí od záložního kondenzátoru.
* **C1 (Kondenzátor):** Elektrolytický, min. 1000µF / 10V (lépe 2200µF). Čím více, tím lépe. Udrží procesor naživu cca 1-2 vteřiny po výpadku, aby stihl uložit čas.
* **R1 + R2 (Dělič):** Rezistory 10kΩ nebo 22kΩ. Slouží ke snížení vstupních 5V na úroveň bezpečnou pro pin A0 (max 3.2V).


## 🔌 Schéma Zapojení - Rozšířené (S UPS)

Toto je vylepšená varianta zapojení. Použijte ji, pokud **potřebujete** funkci zálohování času při výpadku proudu. V nastavení aplikace aktivujte funkci **"Povolit UPS (A0)"**.
                                
```text                                
                                [ ČÁST 1: NAPÁJENÍ A UPS DETEKCE ]

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


                                [ ČÁST 2: PROPOJENÍ LOGIKY ]

                     WEMOS D1 MINI                       L298N DRIVER (H-Můstek)
                  +-----------------+                +---------------------------+
                  |                 |                |                           |
      (viz výše)--| 5V          GND |----------------| GND (Společná zem!)       |
      (viz výše)--| A0           D6 |----------------| IN1 (Minuty A)            |
                  |              D5 |----------------| IN2 (Minuty B)            |
                  |              D0 |----------------| IN3 (Sekundy A)           |
                  |              D7 |----------------| IN4 (Sekundy B)           |
                  |                 |                |                           |
                  |         (SCL) D1|----->[OLED]    | OUT1      OUT2            |
                  |         (SDA) D2|----->[OLED]    |  |          |             |
                  +-----------------+                +--|----------|-------------+
                                                        |          |
                                                    [ Linka Minut 24V ]
                                                    (Hodiny Pragotron)

                                [ ČÁST 3: NAPÁJENÍ CÍVEK ]

      Zdroj 24V DC (+) -----------------------------> L298N svorka +12V/24V
      Zdroj 24V DC (-) -----------------------------> L298N svorka GND
```



## 🔌 Schéma Zapojení - Základní (Bez UPS)

Toto je zjednodušená varianta zapojení. Použijte ji, pokud **nepotřebujete** funkci zálohování času při výpadku proudu. V nastavení aplikace nechte funkci "Povolit UPS (A0)" **vypnutou**.

```text
                                [ NAPÁJENÍ LOGIKY (5V) ]

   Zdroj 5V (USB/DC) (+) ------------------+--------------------------> Wemos 5V
                                           |
                                           +--------------------------> L298N Logic +5V

   Zdroj 5V (USB/DC) (-) ------------------+--------------------------> Wemos GND
                                           |
                                           +--------------------------> L298N GND


                                [ PROPOJENÍ ŘÍZENÍ ]

                     WEMOS D1 MINI                       L298N DRIVER (H-Můstek)
                  +-----------------+                +---------------------------+
                  |                 |                |                           |
      (viz výše)--| 5V          GND |----------------| GND (Společná zem!)       |
      (nezapojen)-| A0           D6 |----------------| IN1 (Minuty A)            |
                  |              D5 |----------------| IN2 (Minuty B)            |
                  |              D0 |----------------| IN3 (Sekundy A)           |
                  |              D7 |----------------| IN4 (Sekundy B)           |
                  |                 |                |                           |
                  |         (SCL) D1|----->[OLED]    | OUT1      OUT2            |
                  |         (SDA) D2|----->[OLED]    |  |          |             |
                  +-----------------+                +--|----------|-------------+
                                                        |          |
                                                    [ Linka Minut 24V ]
                                                    (Hodiny Pragotron)

                                [ NAPÁJENÍ CÍVEK (24V) ]

      Zdroj 24V DC (+) -----------------------------> L298N svorka +12V/24V
      Zdroj 24V DC (-) -----------------------------> L298N svorka GND
```

### ⚡ Důležité poznámky k zapojení
1.  **Společná zem (GND):** Je naprosto kritické, aby `GND` zdroje 24V a `GND` zdroje 5V byly propojeny na svorkovnici modulu L298N (nebo na Wemosu). Bez společné země nebude řízení fungovat a signály budou "plavat".
2.  **Pin A0:** V tomto zapojení zůstává pin `A0` na Wemosu volný (nezapojený). Ujistěte se, že v nastavení je UPS vypnutá, jinak bude systém hlásit falešný výpadek proudu.
3.  **Napájení logiky L298N:** Protože používáte externí 24V pro cívky, **vyjměte** na modulu L298N propojku (jumper) umístěnou nad svorkami napájení (označení `5V-EN` nebo podobné). Následně přiveďte čistých 5V ze zdroje (nebo z Wemosu) do svorky `+5V` na L298N.
    * *Důvod:* Vestavěný stabilizátor na L298N by se při srážení napětí z 24V na 5V extrémně zahříval a mohl by shořet.

---
*Pragotron Master Control © 2025 Miroslav Urban*

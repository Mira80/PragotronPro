# 🕰️ Pragotron Pro - Firmware v8.1.1 (Beta)

**Platforma:** Wemos D1 Mini (ESP8266) + L298N Driver + OLED Shield
**Aktuální verze:** 8.1.1

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

## 🔄 Máte už hodiny doma? (Aktualizace)

Pokud už hodiny používáte a potřebujete jen nahrát novou verzi ze souboru (`.bin`), který jste obdrželi e-mailem, nemusíte nic instalovat ani připojovat kabelem.

👉 **[Přejít na návod: Jak nahrát aktualizaci ze souboru](#-návod-pro-uživatele-jak-nahrát-aktualizaci-ze-souboru)**
  ---

## ✨ Novinky ve verzi v8.1.1 (Beta)

* **📶 Chytré připojení (Smart Connectivity):** Kompletně přepracované nastavení sítě.
    * **WiFi Scanner:** Tlačítko pro automatické vyhledání okolních sítí (není třeba ručně vypisovat SSID).
    * **Dual Mode:** Připojování k domácí WiFi probíhá na pozadí **bez restartu**. Přímo na stránce vidíte živě stav a přidělenou IP adresu.
    * **Zobrazení hesla:** Možnost odkrýt zadávané heslo pro kontrolu před uložením.
* **🛟 Záchranná brzda (Auto-AP Fallback):** Pokud hodiny ztratí spojení s vaší WiFi (např. při změně routeru), systém to rozpozná a automaticky znovu aktivuje vlastní přístupový bod (`Pragotron_AP`).
* **⚡ Hardwarová flexibilita:**
    * **Reverzní polarita:** Možnost softwarově přehodit polaritu výstupů (prohození lichého a sudého pulzu) přímo v nastavení.
    * **Ochrana cívek:** Implementovány "tvrdé limity" pro délku impulzů (max 2000 ms pro minuty, 500 ms pro sekundy), což brání poškození hodin při chybě uživatele.
* **🚀 Optimalizace paměti (Lite Core):** Odstraněním náročných šifrovacích knihoven se uvolnilo obrovské množství RAM. To zajišťuje maximální stabilitu webového serveru a spolehlivou NTP synchronizaci i na slabší WiFi.
* **🛠️ Robustní systém:** Vylepšené ukládání konfigurace a ošetření JavaScriptu proti problémům s mezipamětí (cache) prohlížeče.

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
```
                                                                        ---

## 🆘 Návod pro uživatele: Jak nahrát aktualizaci ze souboru

Pokud jste obdrželi aktualizační soubor e-mailem (koncovka `.bin`), postupujte podle tohoto návodu. K nahrání nepotřebujete žádné kabely, probíhá to bezdrátově.

**Co budete potřebovat:**
* Počítač nebo chytrý telefon.
* Soubor s aktualizací (např. `pragotron_7.8.0.bin`) uložený ve vašem zařízení.

### Krok 1: Připojení k hodinám
Musíte být na stejné WiFi síti jako hodiny.
* **Jste doma?** Ujistěte se, že váš telefon/počítač je připojen ke stejné domácí WiFi, na kterou jste hodiny dříve nastavili.
* **Jste u hodin poprvé nebo nemají WiFi?** V seznamu WiFi sítí ve svém telefonu najděte síť **`Pragotron_AP`** a připojte se k ní.

### Krok 2: Otevření ovládání
1.  Otevřete internetový prohlížeč (Chrome, Safari, Edge...).
2.  Do adresního řádku napište **IP adresu** hodin a potvrďte.
    * *Tip: IP adresu najdete na malém displeji hodin ihned po jejich zapnutí (např. `192.168.1.45`).*
    * *Pokud jste připojeni k `Pragotron_AP`, adresa je vždy:* **`192.168.4.1`**

### Krok 3: Nahrání souboru
1.  V menu hodin klikněte na tlačítko **⚙ Nastavení**.
2.  Sjeďte úplně dolů do sekce **🛠 Údržba**.
3.  Klikněte na modré tlačítko **⬆ Aktualizace Firmware**.
4.  Klikněte na **"Vybrat soubor"** (nebo "Zvolit") a najděte stažený `.bin` soubor ve vašem zařízení.
5.  Klikněte na tlačítko **🚀 NAHRÁT A RESTARTOVAT**.

### Krok 4: Hotovo
Objeví se lišta průběhu. **V tuto chvíli neodpojujte hodiny ze zásuvky!**
Po dokončení (cca 1 minuta) se hodiny samy restartují a naběhnou s novou verzí programu.

> **Jak ověřím verzi?**
> Jděte znovu do menu *Nastavení* -> *Aktualizace Firmware*. Dole uvidíte aktuální datum sestavení (např. `Build: Jan 25 2026`).

# 🕰️ Pragotron Pro - Firmware v8.2.4

**Platforma:** Wemos D1 Mini (ESP8266) + L298N Driver + OLED Shield + UPS Modul
**Aktuální verze:** 8.2.4 (Robust DST & UPS Edition)

Vítejte v oficiálním repozitáři pro **Pragotron Master Control**. Tento projekt promění čip ESP8266 v profesionální řídící jednotku pro podružné hodiny (systém Pragotron/Elektročas) s minutovými nebo sekundovými pulzy.

> **ℹ️ Poznámka:** Tento repozitář slouží k distribuci zkompilovaného firmwaru pro snadnou instalaci přes prohlížeč.

---

## 🚀 Rychlá Instalace (Web Installer)

K nahrání softwaru nepotřebujete Arduino IDE, ovladače ani stahovat binární soubory. Stačí prohlížeč (Chrome/Edge) a USB kabel. Je potřeba instalovat ovladac wemosu, u cinskych modulu je to obvykle ovladac CH340

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

## 🔧 Novinky a vylepšení (Verze 8.2.x)

Tato série aktualizací přináší podporu bateriových záloh a absolutní neprůstřelnost při změně času.

* **☀️ Robustní letní čas (Nezničitelný DST Engine):**
    * Kompletně přepsaná logika pro letní/zimní čas. Systém už nečeká na "jednu konkrétní vteřinu" pro přepnutí, ale neustále matematicky ověřuje platnost letního času.
    * Díky tomu hodiny nepřijdou o změnu času ani v případě, že je Wemos zrovna bez proudu nebo se restartuje.
    * OLED displej, webové rozhraní i funkce Kalibrace nyní plně a správně zohledňují DST posun.
    * **Kalibrace přes půlnoc:** Vylepšený výpočet zajišťuje správné srovnání času i v případě, že kalibrujete hodiny těsně přes půlnoc.
* **🔋 Profesionální UPS ochrana s vizuálním varováním na Webu i OLED:**
    * Systém je nyní plně připraven na napájení z modulu s 18650 článkem.
    * Při výpadku proudu hodiny okamžitě a bezpečně uloží svůj stav do Flash paměti (LittleFS), odpojí cívky pro úsporu energie a přejdou do režimu přežití.
    * OLED displej se probudí a začne svítit varovným nápisem "POWER FAIL".
    * Webové rozhraní (přes mobil) okamžitě zobrazí červený výstražný panel.
* **🧠 Fázová historie (Sudá/Lichá):**
    * V dřívějších verzích hrozilo při opakovaném výpadku proudu nebo hromadném docvakávání přehození polarity (hodiny dostaly dva liché pulzy po sobě a krok se neprovedl).
    * **Oprava:** Wemos si již nepamatuje "poslední polaritu". Místo toho má v sobě naprogramovanou matematickou časovou osu. Před každým pulzem vypočítá: *"Jaký je reálný čas? Kolik pulzů chybí? Tedy, na jaké fyzické minutě teď hodiny stojí?"* Z toho odvodí absolutně správný následující krok.
* **⏳ Paměťový kalendář pro SmartWait a Stop:**
    * Pokud hodiny zrovna stojí (uživatel zapnul na webu pauzu, nebo se čeká až reálný čas "doběhne" hodiny) a vypadne proud, Wemos si tento fakt nově zapamatuje.
    * Při obnově napájení systém odečte plánovanou pauzu od doby trvání výpadku proudu, takže nevznikne žádný deficit zmeškaných pulzů!

----

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
  V aktuální verzi bylo upuštěno od kondenzátoru a ten byl nahrazen 5V UPS modulem s článkem 18650. Při výpadku zdroj se zastaví pulzy, wemos však jede dál z akumulátoru.
  
  https://www.aliexpress.com/item/1005009139076753.html?spm=a2g0o.detail.pcDetailBottomMoreOtherSeller.4.5f93DQvUDQvUmX&gps-id=pcDetailBottomMoreOtherSeller&scm=1007.40050.354490.0&scm_id=1007.40050.354490.0&scm-url=1007.40050.354490.0&pvid=6f8ea4db-ffe3-406d-91f6-461c284b52aa&_t=gps-id:pcDetailBottomMoreOtherSeller,scm-url:1007.40050.354490.0,pvid:6f8ea4db-ffe3-406d-91f6-461c284b52aa,tpp_buckets:668%232846%238110%231995&pdp_ext_f=%7B%22order%22%3A%22597%22%2C%22eval%22%3A%221%22%2C%22sceneId%22%3A%2230050%22%2C%22fromPage%22%3A%22recommend%22%7D&pdp_npi=6%40dis%21CZK%2154.14%2147.18%21%21%212.49%212.17%21%40211b81a317747779153341702ecea7%2112000056589717493%21rec%21CZ%21193171986%21XZ%211%210%21n_tag%3A-29919%3Bd%3A7c1950ac%3Bm03_new_user%3A-29895&utparam-url=scene%3ApcDetailBottomMoreOtherSeller%7Cquery_from%3A%7Cx_object_id%3A1005009139076753%7C_p_origin_prod%3A

```text
                                [ NAPÁJENÍ A UPS DETEKCE ]

 [ ZAPOJENÍ S UPS MODULEM A BYPASS DIODOU ]
Napájeni 5V jde do UPS modulu, zároveň přes schottky diodu napájí wemos.
Přes odporový dělič je vstupní napětí před diodou připojeno na A0
Výstup UPS modulu je napojeno na wemos.
Při výpadku napájení je ztráta detekována pomocí A0, díky UPS však wemos funguje dál

       Vstup 5V (USB/Zdroj)
         |
         +-----------+----------------------+
         |           |                      |
         |          [R1] (Rezistor 10k)     |
         |           |                      |
         |           +-------------------------------> PIN A0 (Wemos)
         |           |                      |          (Měří napětí PŘED diodou)
         |          [R2] (Rezistor 15k)     |
         |           |                      |
         |          GND                     v (IN+ je PŘED diodou)
         |                        +-------------------+
         |                        |        IN+        |
         |                        |     UPS modul     |
         |                        | (18650 Baterie)   |
         |                        |              OUT+ |----+-------------------> Wemos 5V
         |                        |                   |    |                 
         |                        | IN-          OUT- |    | (OUT+ je ZA diodou)             
         |                        +--+-------------+--+    |                 
         |                           |             |       | 
         +-------->| D |-------------|-------------|-------+                 
                  (Dioda)            |             |                         
                                     |             |                         
         GND ------------------------+-------------+---------------------------> Wemos GND
                                                                             
                                                                             (Propojit s GND na L298N)
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

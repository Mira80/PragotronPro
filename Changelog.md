# Changelog

## [v7.5.0] - 2025-12-27
### Final UX Update & Stability Improvements

Tato verze se zaměřuje na výrazné zlepšení uživatelského zážitku (UX) při konfiguraci zařízení a eliminuje chyby spojené s restartem webového serveru.

### 🚀 Novinky (Features)
* **Elegantní Restart:** Implementován nový mechanismus ukládání konfigurace. Místo okamžitého přerušení spojení (chyba `ERR_CONNECTION_RESET`) server nyní odešle potvrzovací HTML stránku a restart provede s bezpečnou prodlevou.
* **Visual Countdown:** Přidána obrazovka s odpočtem času (20 sekund) během restartu, která automaticky obnoví stránku po náběhu systému.
* **Předvyplňování Nastavení:** Formuláře v sekci Nastavení (WiFi, Identifikace, Čas) se nyní automaticky předvyplňují aktuálními hodnotami uloženými v EEPROM/FS.
* **Status v Záhlaví:** Název umístění (např. "Obývák") se nyní zobrazuje v záhlaví všech stránek pro lepší orientaci při správě více zařízení.

### ✨ Vylepšení (Improvements)
* **Bezpečnější WiFi formulář:** Pole pro heslo WiFi je nyní typu `password` (znakové hvězdičky) místo prostého textu.
* **Prodloužený Timeout:** Časovač pro obnovení stránky po restartu byl navýšen na **20 sekund**, což zajišťuje spolehlivé načtení i na pomalejších routerech.
* **Tlačítka:** Přehlednější popisky tlačítek v nastavení ("ULOŽIT A RESTARTOVAT"), které jasně indikují následnou akci.
* **API:** Rozšířen endpoint `/get_config` o parametry `ssid`, `pass` a `host` pro potřeby frontendového předvyplňování.

### 🐛 Opravy (Bug Fixes)
* **FIX:** Odstraněna chyba, kdy se po uložení nastavení prohlížeč "zasekl" na chybové stránce o přerušení spojení.
* **FIX:** Opraveno chování odpočtu, který v určitých případech mohl počítat do záporných hodnot (nyní se zastaví na 0 a čeká).
* **FIX:** Opraveno prázdné pole `Hostname` při vstupu do nastavení, které nutilo uživatele zadávat název znovu.


**Kompatibilita:**
* HW Platforma: Wemos D1 Mini (ESP8266)
* Display: OLED 0.66" Shield (SSD1306)
---

## [v7.4.0] - 2025-12-26
### Kompletní přepracování původního kódu z roku 2021


**Kompatibilita:**
* HW Platforma: Wemos D1 Mini (ESP8266)
* Display: OLED 0.66" Shield (SSD1306)
 

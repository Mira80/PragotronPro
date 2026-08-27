# Changelog

## [v8.2.5] - 2026-08-27
Oprava chyby, kdy horší konektivita při startu způsobila posun zobrazeného času o 30 min
Oprava chyby webového rozhraní, kdy se nezobrazila požadovaná webová stránka

## [v8.1.4] - 2026-01-31
### Stable Lite & Logic Repair Edition

Klíčová verze zaměřená na maximální stabilitu paměti (RAM), opravu logiky řízení hodin a nové možnosti připojení. Odstraněna podpora HTTPS pro zvýšení spolehlivosti.

### 🚀 Nové funkce (New Features)
* **Smart Connectivity (WiFi Scanner):** Kompletně nové rozhraní pro připojení.
    * **Scanner:** Tlačítko pro vyhledání okolních sítí (není třeba ručně psát SSID).
    * **Live Connect:** Připojení k domácí WiFi probíhá na pozadí bez nutnosti restartu.
    * **Show Password:** Možnost zobrazit zadávané heslo.
* **Auto-AP Fallback (Záchranná brzda):** Pokud hodiny ztratí spojení s WiFi (nebo se změní router), systém to pozná a automaticky znovu aktivuje vlastní AP (`Pragotron_AP`).
* **Hardwarová Flexibilita:**
    * **Reverzní polarita:** Možnost softwarově otočit řízení cívky (sudá/lichá) v nastavení.
    * **Ochrana cívek:** Implementovány tvrdé limity délky impulzu (max 2000ms), aby nedošlo k přehřátí cívky při chybě uživatele.

### ⚡ Optimalizace (Optimizations)
* **Lite Core (RAM Boost):** Odstraněny náročné šifrovací knihovny (SSL/HTTPS). Webové rozhraní je díky tomu výrazně rychlejší a stabilnější, nedochází k pádům `HTTP -1` (Out of Memory).

### 🐛 Kritické Opravy Logiky (Logic Fixes)
* **FIX: Tichý Start (Silent Start):** Opravena chyba, kdy hodiny ihned po startu (při získání času z NTP) vyslaly jeden falešný impulz navíc. Nyní první čas pouze uloží do paměti.
* **FIX: Zastavení bez dohánění (No-Catch-Up):** Při aktivaci funkce *Manuální Zastavení* (STOP) se již pulzy nepřičítají do fronty. Po odblokování hodin tak nedochází k nechtěnému "docvakávání" zameškaného času.

---
## [v7.8.0] - 2026-01-15
### Zero-Loss Logic & Build Info

* **🛡️ Robustní řízení minut (Zero-Loss Logic):** Zcela přepsaná logika generování impulzů. Systém již nečeká na "nultou sekundu", ale aktivně sleduje změnu času. Tím je zaručeno, že hodiny **nevynechají minutu** ani při krátkodobém zaseknutí WiFi nebo synchronizaci NTP.
* **📅 Datum sestavení (Build Time):** V menu *Aktualizace* a v patičce *Nápovědy* se nyní zobrazuje přesné datum a čas kompilace firmwaru.
* **🎨 Refined Design:** Drobná vylepšení CSS stylů pro "Flip-clock" vzhled.

---
## [v7.6.1] - 2025-12-28
### Captive Portal & Retro UI Edition

Výrazné rozšíření funkčnosti o automatický přihlašovací portál a vylepšení vizuálního stylu webového rozhraní.

### 🚀 Nové funkce (New Features)
* **Captive Portal (WiFi Manager):** V režimu přístupového bodu (AP) zařízení nyní automaticky zachytává síťový provoz. Po připojení telefonu k `Pragotron_AP` se automaticky nabídne přihlašovací stránka (funguje jako hotelová WiFi), což eliminuje nutnost ručně zadávat IP adresu `192.168.4.1`.
* **Časovač Zastavení (Stop Timer):** U manuálního zastavení hodin lze nyní nastavit dobu v minutách (např. 60 min). Po uplynutí této doby se hodiny automaticky znovu spustí. V přehledu se zobrazuje odpočet zbývajícího času.
* **Retro Design:** Záhlaví webové stránky bylo přepracováno do stylu překlápěcí tabule (Flip-dot/Split-flap) s větším písmem, které lépe odpovídá charakteru zařízení.
* **Chytré Ořezávání SSID:** Na OLED displeji se nyní dlouhé názvy WiFi sítí automaticky zkracují (např. "TatoJeMoje..." místo přetečení na další řádek).
* **Dynamický Titulek:** Název záložky v prohlížeči nyní obsahuje nastavené umístění (např. "Obývák - Pragotron PRO").

### 🐛 Opravy a Vylepšení
* **FIX:** Tlačítka ovládání (Ruční posun, Kalibrace) nyní správně indikují zablokovaný stav (červená barva, nápis "HODINY ZASTAVENY"), pokud je aktivní manuální STOP nebo časovač.
* **UX:** Zvětšeno písmo v záhlaví pro lepší čitelnost na mobilních zařízeních.

---
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
 

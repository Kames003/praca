
# Ako zistiť, či si pripojený na vzdialený server cez SSH

## 1. Skontroluj premennú prostredia `SSH_CONNECTION`
```bash
whoami && echo $SSH_CONNECTION
```
- **Význam:**
  - `whoami` zobrazí tvoje aktuálne používateľské meno.
  - `$SSH_CONNECTION` obsahuje informácie o SSH spojení.
- **Ak je výstup `$SSH_CONNECTION` prázdny**, **nie si pripojený cez SSH**.

---

## 2. Skontroluj premennú prostredia `SSH_CLIENT`
```bash
echo $SSH_CLIENT
```
- Táto premenná tiež obsahuje informácie o SSH spojení (IP klienta, porty).
- **Prázdna hodnota** znamená, že **nie si v SSH relácii**.

---

🧠 **Poznámka:** Tieto premenné sú nastavené **iba počas SSH pripojenia**, takže ak ich výstup neobsahuje žiadne údaje, si **lokálne na svojom stroji** a **nie na vzdialenom serveri**.



# 🧠 Fun Fact: Čo je GNOME?

**GNOME** je **grafické používateľské rozhranie (GUI)** pre Linux – teda tá časť systému, ktorú **vidíš** a **s ktorou pracuješ pomocou myši**.

---

## 🖼️ GNOME v skratke

- **GNOME** je súčasťou väčšiny Linuxových distribúcií ako grafické prostredie.
- Umožňuje spúšťať aplikácie, ovládať okná, spravovať súbory a systém.

---

## ⚙️ Ako GNOME funguje?

- Beží na **X11** alebo **Wayland**
  - To sú **zobrazovacie servery**, ktoré **komunikujú medzi hardvérom a grafickým prostredím**.
- Používa **GTK** – vlastný **toolkit**:
  - Je to súbor knižníc určený na **tvorbu aplikácií** (napr. GNOME Files, Terminal, Settings).

---

💡 GNOME je predvolené prostredie pre **Fedora Workstation** a mnoho ďalších distribúcií.


--- 

Apps otváraj v štýle napr. gnome-control-center == otvori nastavenia 

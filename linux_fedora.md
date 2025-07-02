
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

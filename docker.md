# Toto sa vzťahuje na docker app 
# Čo je to container (kontejner)?

**Jednoducho povedané:**  
Kontajnery sú *izolované procesy*, v ktorých bežia jednotlivé komponenty tvojich aplikácií.

---

## Vlastnosti kontajnera:

- **Izolované prostredie:**  
  Každý kontajner beží vo vlastnom priestore, *úplne oddelene* od ostatných kontajnerov aj hostiteľského systému.

- **Samostatnosť (self-contained):**  
  Obsahuje všetko, čo aplikácia potrebuje (kód, knižnice, runtime).  
  **Nie je závislý** na tom, čo je nainštalované v host systéme.

- **Bezpečnosť:**  
  Vďaka izolácii má *minimálny vplyv* na hosta aj na iné kontajnery.  
  Znižuje riziko narušenia bezpečnosti.

- **Manažovateľnosť:**  
  Každý kontajner je *samostatne spravovateľný* – jeho odstránenie neovplyvní ostatné.

- **Prenositeľnosť (portability):**  
  Môže bežať **kdekoľvek** – na vývojovom stroji, v dátovom centre, v cloude...  
  Rovnaký výsledok na rôznych prostrediach = *"works on my machine"* problém vyriešený.

---

## Containers vs Virtual Machines (VMs)

| Vlastnosť          | Container                           | Virtual Machine (VM)                     |
|--------------------|--------------------------------------|------------------------------------------|
| **Jadro systému**  | Zdieľa jadro s hostom                | Má vlastné jadro                         |
| **Overhead**       | Nízky – je to len proces             | Vysoký – celé OS                         |
| **Izolácia**       | Izolovaný proces                     | Celý izolovaný systém                    |
| **Efektivita**     | Veľa kontajnerov na jednej infra    | Viac VM = väčšia spotreba zdrojov       |
| **Rýchlosť štartu**| Sekundy                              | Minúty                                   |
| **Prenositeľnosť** | Vysoká (build once, run anywhere)    | Nižšia, závislá od hypervízora           |

---

## Zhrnutie

- Kontajner je ľahký, izolovaný balík obsahujúci všetko potrebné pre spustenie aplikácie.
- Umožňuje bežať viacero aplikácií efektívne na jednej infraštruktúre.
- Je rýchly, bezpečný, prenosný a jednoducho sa spravuje.


# Git vs GitLab – Komplexné Poznámky

## Základné rozdiely

### Git
- **Command line tool** na správu verzií (version control).
- Umožňuje **lokálne** sledovať zmeny v kóde.
- Pracuješ s vetvami, commitmi, spájaním zmien atď.
- Vytvára lokálny **snapshot** aplikácie.
- Každý commit má jedinečný ID hash.
- Predvolená vetva po inicializácii je `master` alebo `main`.

### GitLab
- **Webová platforma** postavená nad Gitom.
- Umožňuje **centrálne hostovanie** Git repozitárov.
- Podporuje **spoluprácu v tíme**, **CI/CD pipeline**, **issue tracking**, **wiki**, a **merge requesty**.
- Hosting na: `https://gitlab.com/`, prípadne na firemnom servery (napr. `gitlab.railsformers.com`).

---

## Lokálna práca s Gitom

```bash
# Zobrazenie vetvy
git branch

# Pridanie súboru na commitovanie
git add alpha.txt

# Pridanie všetkých zmien v priečinku
git add .

# Uloženie zmien do histórie repozitára
git commit -m "Tvoja správa"

# Skombinovanie git add + commit pre už sledované súbory
git commit -a -m "updated echo"

# Prehľad commitov
git log --oneline
git log --all --oneline --decorate --graph
```

---

## Vzdialený repozitár (origin)

```bash
# Pridanie zmien do vzdialeného repozitára
git push

# Načítanie zmien od iných členov tímu
git pull

# Zobrazenie URL repozitára
git remote -v
```

`origin` = názov vzdialeného servera (napr. GitLab).

---

## Fetch vs Pull

- `git fetch` = Stiahne dáta zo vzdialeného servera, ale **neaplikuje** ich hneď.
- `git merge` = Aplikuje zmeny z fetchnutého obsahu do tvojej aktuálnej vetvy.
- `git pull` = Kombinácia `fetch` + `merge`.

---

## Reset a história

```bash
# Vracia do stavu určitého commitu a vymaže zmeny
git reset --hard <commit_hash>

# Vrátenie posledného commitu
git reset --hard

# Zistenie histórie referencií
git reflog
```

Pozor: po `--hard` resete môžu byť commity **nenávratne stratené**, pokiaľ nie sú uložené vzdialene.

---

## Cherry-pick

```bash
# Skopíruje konkrétny commit z inej vetvy do aktuálnej
git cherry-pick <commit_hash>
```

---

## Práca s vetvami

```bash
# Vytvorenie novej vetvy
git branch dev

# Prepnutie na vetvu
git switch dev
# Alebo: git checkout dev

# Vytvorenie a prepnutie naraz
git switch -c dev

# Zoznam vetiev
git branch --list
```

### Vizuálne zobrazenie:
```
* 35ca233 (HEAD -> master) this is the fourth commit
| * 3f5b5a8 (dev) third one
| * 572057b third one
|/  
* c2f80a7 First commit
```

---

## Merge vs Rebase

- **Merge**: Spája vetvy a zachováva **históriu**.
- **Rebase**: Prepisuje históriu a vkladá commity "ako keby" vznikli na novej báze.

---

## Práca so súbormi

```bash
# Vytvorenie súboru (alebo update timestampu ak už existuje)
touch example.txt
```

---

## SSH prihlásenie (bez zadávania hesla)

```bash
# Vygenerovanie kľúča
ssh-keygen -t ed25519 -C "tomasmucha@railsformers.com"

# Zobrazenie public key
cat ~/.ssh/id_ed25519.pub
```

➡️ Tento public key pridaj do GitLab:
- GitLab → Settings → SSH Keys → vlož tam výstup z `cat`.

---

## Ďalšie tipy

- Ukončenie editoru `nano`: `CTRL + X`
- Ukončenie `vim`: `ESC`, potom `:q!` alebo `:wq`

---

## Push projektu do GitLab

```bash
git init
git remote add origin https://gitlab.railsformers.com/tomas.mucha/my-project.git
git add .
git commit -m "initial commit"
git push -u origin main
```

---

## PAT – Personal Access Token

Používa sa namiesto hesla pri HTTPS prístupe do GitLab, najmä ak je dvojfaktorová autentifikácia zapnutá.

---

**Poznámka**: `origin/main` alebo `origin/master` je názov vetvy vo vzdialenom repozitári.

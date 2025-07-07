# Docker Základy a Architektúra

## Architektúra Dockeru

- **Klient – démon**: `docker` klient posiela príkazy démonovi `dockerd`, ktorý ich vykonáva.
- **Docker Daemon** (`dockerd`): backendová služba, ktorá spravuje objekty ako kontajnery, imagy, volume atď.
- **Docker CLI**: príkazový riadok na interakciu s Dockerom.
- **Docker Registry**: sklad pre Docker imagy – napr. Docker Hub.

## Objekty Dockeru

### Image

- Nemenná šablóna aplikácie (blueprint).
- Obsahuje OS vrstvu, aplikáciu a jej závislosti.
- Príklad vytvorenia: `docker build -t myapp .`

### Container

- Spustiteľná inštancia Docker imagu.
- Má vlastný súborový systém, procesy, siete.
- Je izolovaný a ľahký.
- Vzniká pomocou: `docker run <image>`

### Dockerfile

- Skript, ktorý automatizuje tvorbu Docker imagov.

### Volumes

- Persistentné úložisko dát pre kontajnery.
- Sleduj **lifecycle**, anonymné volume, blind mounts.
- Syntax: `--mount`, `-v` pre volume mountovanie.

## Práca s Dockerom

### Základné príkazy

```bash
docker run hello-world          # spustí testovací kontajner
docker images                   # vypíše dostupné imagy
docker ps -a                    # všetky kontajnery
docker logs <id>                # logy kontajnera
docker stop <id>                # zastavenie kontajnera
docker start <id>               # spustenie kontajnera
docker rmi <image_id>           # zmazanie imagu
```

### Detached mode

```bash
docker run -d redis             # spustenie v pozadí
```

### Port Binding

```bash
docker run -p <host_port>:<container_port> <image>
# napr: docker run -p 6000:6379 redis
```

- Bez bindovania nie je port kontajnera prístupný zvonka.
- Príklad výstupu: `0.0.0.0:6000->6379/tcp` znamená host port 6000 je spojený s container portom 6379.

## Porovnanie s Virtual Machines

| Feature             | Docker Container                   | Virtual Machine                    |
|---------------------|------------------------------------|------------------------------------|
| Kernel              | Zdieľaný s hostom                  | Vlastný kernel                     |
| Veľkosť             | Pár MB                             | GB                                 |
| Spustenie           | Sekundy                            | Minúty                             |
| Izolácia            | Procesová (namespace/cgroups)      | Plná VM úroveň                     |

## Technológie na pozadí

- **Namespaces** – izolácia procesov, sietí, filesystemu atď.
- **Control groups (cgroups)** – obmedzenie zdrojov pre kontajnery (CPU, RAM...).
- **Union File System (napr. OverlayFS)** – spája vrstvy do jedného pohľadu.
  - Výhoda: sťahujú sa len rozdielne vrstvy medzi verziami.

## Motivácia použiť Docker

- Štandardizovaný, ľahký, izolovaný balíček aplikácie.
- Prenositeľnosť medzi systémami (vývoj, test, produkcia).
- Jednoduchá správa závislostí.
- Rýchle buildovanie a nasadenie.

## Analógie

- **Docker Hub** = kuchárska kniha (recepty)
- **Docker Image** = konkrétny recept
- **Docker Container** = hotové jedlo

# Docker: Rozdiel medzi `docker run`, `docker images` a `docker start`

## 🔹 docker run

- **Vytvára a spúšťa nový kontajner** na základe daného imagu.
- Ak image nie je lokálne, automaticky sa stiahne z Docker Hubu.
- Pomocou parametrov vieš hneď definovať:
  - `-p <host>:<container>` – mapovanie portov
  - `-d` – spustenie v pozadí (detached mode)
  - `--name` – pomenovanie kontajnera
  - `-v` – mountovanie volume
  - `-e` – nastavenie environmentálnych premenných

### Príklad:
```bash
docker run -d -p 8080:80 --name webserver nginx
```
➡️ Vytvorí a spustí nový kontajner `webserver` z imagu `nginx`.

---

## 🔹 docker images

- **Zobrazí zoznam všetkých imagov**, ktoré sú uložené lokálne.
- Nepretraktuje kontajnery, len samotné imagy (blueprinty).
- Image môžeš zmazať pomocou `docker rmi <image_id>`.

### Príklad:
```bash
docker images
```

#### Výstup:
```
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
nginx        latest    abc123def456   2 weeks ago     133MB
```

---

## 🔹 docker start

- **Spúšťa už existujúci kontajner**, ktorý bol predtým vytvorený cez `docker run`.
- Zachová všetky pôvodné atribúty kontajnera (porty, mounty, názvy...).
- Vhodné na opätovné spustenie po vypnutí.

### Príklad:
```bash
docker start webserver
```

#### Interaktívne so shellom:
```bash
docker start -ai webserver
```

---

## 🧠 Zhrnutie:

| Príkaz         | Význam |
|----------------|--------|
| `docker run`   | Vytvorí a spustí **nový kontajner** s definovanými parametrami |
| `docker images`| Vypíše všetky **lokálne dostupné imagy** |
| `docker start` | Spustí **už existujúci** (zastavený) kontajner |
- **Docker Engine** = kuchár, ktorý varí podľa receptu

## 🧩 Príkazy

### Spustenie Shellu v kontajneri
```bash
docker exec -it <container_id_or_name> /bin/bash
```
- `-i` (interactive): udrží štandardný vstup otvorený – môžeme písať do terminálu
- `-t` (tty): priradí pseudo-terminál (simuluje terminálové rozhranie)

> Týmto sa dostávame do interaktívneho prostredia kontajnera. Každý kontajner má svoj izolovaný virtuálny súborový systém.

### Zobrazenie environmentálnych premenných
```bash
printenv
```

---

## 🚀 `docker run`

- Vytvorí a spustí nový kontajner zo zadaného imagu
- Príklad:
```bash
docker run -d -p 27017:27017 mongo
```
- `-d`: detached mód (beží na pozadí)
- `-p`: prepája porty `host:container`

---

## 🌐 Docker Networks

- Docker automaticky vytvára izolované siete
- Kontajnery v tej istej sieti spolu komunikujú cez **meno kontajnera**, nepotrebujú `localhost` ani port
- Príkazy:
```bash
docker network ls
docker network create <nazov_siete>
```

### Využitie v praxi:
- Kontajnery (napr. MongoDB a Mongo Express) komunikujú cez mená
- Aplikácie mimo tejto siete (napr. host Node.js) používajú `localhost:port`

---

## ⚙️ Docker Compose vs Docker Run

| Docker Compose                         | Docker Run                             |
|---------------------------------------|----------------------------------------|
| .yaml súbor pre definovanie služieb   | Príkazy v CLI                          |
| Jednoduchšia správa viacerých služieb | Vhodné pre jednoduché testovanie       |
| Verzia zistíš príkazom:               |                                        |
```bash
docker compose version
```

---

## 📝 Zhrnutie

- `docker exec -it`: interaktívny prístup ku kontajneru
- `docker run`: spustenie kontajnera s parametrami
- Docker networks: izolované prostredie pre komunikáciu kontajnerov
- Docker Compose: YAML-based orchestrácia kontajnerov

JS application which connets to the database displays retrieved datas, than we are going to conternize with dockerfile run it as a part of a compose 

server js is the node js backend 

# Important concept 

the way how can override is using a flag in the docker compose 

docker compose --project-name projects -f mongo-services.yaml up -d


toto je overrid predvoleneho nazvu 

with the enviromental variables there are security problems 

docker compose has the concepts of the secrets 

docker images -q = dostaneme len ids 

.json, key value pares fundomentalnym zakladom 
== key musi byt v uvodzovkach 
"name" : "The ultimate Docker Course",
"price": 149,
"tags": 

parsing of the yaml file is little bit slower than parsing of the json file 

just some regular fun facrs docker-compose = 2014, docker-compose version 2 : 2020

V2 ignores top level component teda verziu samotnu 

motivacia prečo sa vůbec compose pouziva ? 

local development, run demo = v)čšina open source projektov poskytuje demičko ktore si můžeme stiahnut a runnut u seba, deploy on a single linux server 

klasik pre python treba virtualne prostredie venv
python -m venv .venv 
source .venv/bin/activate = toto je pre aktivaciu 

pip freeze > requirements.txt = vypiše aktualne nainštalovane python baliky v aktivnom prostredi aj s ich verziami 

! nezabudni na toto je to podstatne 

kde > znamena presmeruj vystup priakzu do súboru 

a samotny .txt slúži ako zoznam zavisloti 

curl 127.0.0.1:5000/about

// we are getting hardcoded version back 

flask --app app run 

--app app = hovoriš moja aplikacia je v súbore app.py 
run = flask spusti vstavany vyvojovy webserver 

flask spúšťa tvoju flask aplikaciu 

## DockerFile how does it should looks like ? 

FROM python:3.12.4-alpine3.20
# Pouzijeme predpripraveny baliček s pythonom 
# začni s oficialnym balikom pythonu 
# verzia pythonu : 3.12.4 

# we use this to reduce the size of Docker images 

RUN apk --no-cache add curl 

if we want to add new packages we need to do it inside of the venv 

containers which are isolated groups of proceses

kazdy container ma všetko na to aby mohol runnovat 
# --no-cache nezachovava cache baličkov čo znižuje velkost docker imagov
# curl je teda nastroj na http poziadavky 

WORKDIR /app

# ? workdir je ako cd / app vo vnutri kontajnera, 
# ! všetky relativne cesty sa vykonavaju v ramci priečinka app 
# od teraz pracuj v priečinku / app v containery 
# bude to v jednom priečinku nebude ziadny bordel 


COPY requirements.txt .

# vezme subor requirements a kopne to do priečinka /app 
# chceme nainštalovať najskor kniznice, a ak sa to nezmeni docker ho vie vyuzit z cache 
# netreba inštalovať všetko znovu = šetrime tym čas pri builde 



RUN pip install -r requirements.txt
# prečitaj req... a nainštaluj všetky potrebne knižnice 



# ! prečíta a nainštaluje baliky do kontajnera 
# ? ak sa requirements.txt nezmenil, docker pouzije cache a preskoči build 

COPY app.py .

# a v poslednom rade zoberiem kod do pripraveneho prostredia kde uz python kniznice su 

# ! posledny krok, ktory určuje čo sa musi spustit ked sa kontajner naštartuje 


CMD ["gunicorn", "--bind", "0.0.0.0:8080", "app:app"]

# gunicorn = produkčny webserver pre python
# bind 8000 aby počuval na všetkych sietovych rozhraniach 

VM = virtual machine emulates entire machines kernel 
container 

by default containers are isolated from the host network 

image size matters a lot 

alpine image is focused on a minimalism

debuging containers 

docker exec -it 196aac69f495 /bin/bash

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
- **Docker Engine** = kuchár, ktorý varí podľa receptu

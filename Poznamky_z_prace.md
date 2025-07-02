# bitov Poznámky 

## ECDSA = Eliptic curve digitacal algorithm = modernejši bezpečnejši než RSA / DSA 

Security strength, udáva sa v bitoch for example 128 bitova bezpečnosř 

Prakticky to znamena ze utočnik musi vyskušat 2 na 128 moznosti aby sa mu podarilo prelomit šifru, tzv brute force 

**Čim viac** bitov tym je naročnejšie a tažšie to prelomit 
Algorithms je viac... ako napr. RSA s klucom 2049-bit 112-bit security 
z
ECDSA velkost kluča je 521-bit a priblizna bezpecnostna uroven je 256 

ECDSA-521 - jedno z najlepšich riešeni, vysoka bezpečnost, rychle podpisovanie a overovanie, efektivnost = maly kluc, maly podpi,
kompatibilita s modernymi systemami napr. gitlab, openSSH

kedy nie ? 
stare systems ( stare ssh servery 
kompatibilita so všetky = rsa je sice slabši ale ma najlepšiu kompatibilitu 

# Kryptograficke algoritmy 

RSA 

zalozena na faktorizacii velkych prvočisel 

  - výhody : vysoka kompatibilita
  - nevýhody : pomalý, vyžaduje velmi dlhe kluce napr.
ECDSA

zalozena na eliptickych krivkach, pracuje s tzv. diskretnym logaritmom nad eliptickou krivkou 

vyhody : kratšie kluče, ale vyššia bezpečnost nez rsa, rychlejši popis a verifikacia 

ED25519 
totozne elipcticke krivky, ale s modernou optimalizaciou pre rychlost a bezpecnot 

# Overenie nainštalovania openssh klienta 

**ssh -V ** 

# Vytvorenie ECDSA-521 SSH Kľúča

ssh-keygen -t ecdsa -b 521 -C "tvoje_meno@tvoj_email"

kde : -t = určuje type kluča ktory sa ma vygenerovat, to su prakticky tie protokoly : rsa, ecdsa, ed25519, dsa )
-b = bitová dlzka kluča 521 - teda pouziva krivku nistp521 

-C = je komentar, nejedna sa o povinny parameter 

# Kroky vytvorenia 

1. ssh-keygen -t ecdsa -b 521 -C "tvoje_meno@tvoj_email"
2. Enter file in which to save the key (/home/tomasmucha/.ssh/id_ecdsa): 
3. Enter the Passphrase je heslo, chrani sukromny kluc ktory sa ulozil na disku, sekundarny ochranny kluc
     - napr keby niekto čorkol id_ecdsa bez hesla sa k tomu nedostane 

# Čo je to asymetricka kryptografia ? 

- šifrovanie kde sa pouzivaju dva rozdielne, ale matematicky prepojene kluce
- funguje to principom to čo zašifruješ jednym klučom, možeš dešifrovat len tym druhym 

sukromny = podpisovanie, dešifrovanie  ===== musi ostat tajny 
verejny = overenovanie podpisu alebo šifrovanie ====== nie moze byt zverejneny 

Prečo to pouzivame ? 

Autentifikacia - systems like gitlab, ssh server - overi ze si to ty bez potreby hesla 
Bezpečnost - aj keby komunikacia bola odchytena, nedokaze ju zneuzit lebo nema private key 
Odstranenie hesiel = hesla su slabe, opakovane pouziavane, nachylne na phisisng, keys su omnho tazšie na prelomenie 

cokolvek čo podpišeme sukromnym klucom moze byt jedinečne overene tym verejnym 
opačne to však nejde 

priklad z real life 
verejny kluc = zamok 
sukromny = kluc 

# Prikaz pre zobrazenenie verejneho kľúča = vdaka nemu sme ho potom schopny skopirofvař 

cat ~/.ssh/id_ecdsa.pub

Kde : cat - concatinate - vyuzitie hlavne na zobrazenie obsahu v subore 
~/.ssh/id_ecdsa.pub = prakticky sa jedna len o cestu k filu
~ skratka pre domaci adresar usera 
.ssh = skryte priečinky v linuxe prave tie ktore začinaju bodkou 
id_ecdsa.pub = subor ktory obsahuje verejny kluc preto tam je .pub 

## pridanie ssh kluča na gitlabe 

Ikona - preferences - add ssh key 

# 3. Podľa navodu v repo ssh-config nahod konf ssh klienta 

cd 
mkdir -p .ssh/shared = vytvorí sa adresar, kde bude ulozeny zdielany konfigurak - napr. definicie pre servery, identiny atď. 

**This is just a comment** 
-p teda zaistuje ze adresar sa vytvori i ked teda .ssh ešte neexistuje 

grep -qxF 'include shared/config' .ssh/config || echo -e '\n\ninclude shared/config' >> .ssh/config
pridanie include do hlavneho /.ssh/config 

teda inymi slovami zabezpečuje to ak v .ssh/config nie je riadok include shared/config tak sa tam prosto addne 

3. Klonovanie zdielaneho repozitaru

cd .ssh/shared
git clone git@gitlab.railsformers.com:railsformers/ssh-config.git .






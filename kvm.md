lsmod | grep kvm  = či su načitane moduly jadra pre virtualizaciu 

modinfo kvm | head

QUEMU vyber 

lscpu | grep -i virtualization = determinizácia čo podporuje 

sudo virt-host-validate qemu 

tuned = čo to je a k čomu nam sluzi ? 

omasmucha@RF-TOMASMUCHA-NTB:~/Plocha/library-app$ sudo systemctl enable --now tuned 
[sudo] heslo pre používateľa tomasmucha: 
tomasmucha@RF-TOMASMUCHA-NTB:~/Plocha/library-app$ tuned-adm active
Current active profile: balanced

tuned-adm list = výpis všetkeho z listu 

sudo tuned-adm profile virtual host = prepnutie 

sudo tuned-adm verify  = verifiakcia hosta 

sudo virsh net-list --all

## Prihlasovačky do kvm na rails pc 

Meno pc : debian
Heslo uzivatela root : 0000
Meno nového usera : new_user 
heslo pre noveho uzivatela : 0000 

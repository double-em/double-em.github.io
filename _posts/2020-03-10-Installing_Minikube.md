---
layout: post
title:  "Guide til Installation af Minikube på Fedora 31"
author: Mike Meldgaard
date:   2020-03-10 #01:00:12 +0530
category: DevOps
summary: Installering og opsætning af Minikube, samt kørsel.
thumbnail: learn.png
---
Dette er en guide til hvordan man får Minikube(Lokal Kubernetes) til at køre på en Linux Fedora.

### Installing Minikube on Fedora ###

1. **Opdater Fedora**
    * `sudo dnf update`

2. **Installer Docker**

    * Ændre cgroups til 1 (Første metode)
        * Installer din foretrukne <code>text-editor</code> (Jeg bruger nano her)
        `sudo dnf install nano`

        * Rediger grub filen
        `sudo nano /etc/default/grub`

        * Tilføj **systemd.unified_cgroup_hierarchy=0"** til enden af **GRUB_CMDLINE_LINUX="..."** linjen.

        * Din grub fil skulle så gerne have en linje der ligner denne
        `GRUB_CMDLINE_LINUX="resume=UUID=... rhgb quiet systemd.unified_cgroup_hierarchy=0"`
    
    * Ændre cgroups til 1 (Anden metode)

        * Installer grubby
        `sudo dnf install grubby`

        * Ændre cgroups med
        `sudo grubby --update-kernel=ALL --args="systemd.unified_cgroup_hierarchy=0"`

        **OBS:** Anden metode med grubby afspejler sig nødvendigvis ikke i selve **/etc/default/grub** - filen og jeg har erfaret at den ikke altid træder i kraft efter første genstart. Her kan man tjekke ens maskine med **virt-host-validate** for at se om nogle punkter fejler. Hvis der er nogle punkter der er røde og siger "failed", så prøv at køre kommandoen igen og genstart. Umiddelbart ligner er der ingen problemer efter man har kørt kommandoen anden gang og genstartet, dog er årsagen til fejlen ikke kendt.

    * Tjek om maskinen bruger EFI/UEFI eller BIOS/Legacy boot med bash
    `[ -d /sys/firmware/efi ] && echo UEFI || echo BIOS`
    Denne kommando / lille script tjekker om **/sys/firmware/efi** - mappen eksitere. Hvis den gør så er maskinen startet med EFI/UEFI boot, hvis ikke er den startet med BIOS boot.
    
    * Lav grub config (EFI/UEFI boot)
    `sudo grub2-mkconfig > /boot/efi/EFI/fedora/grub.cfg`

    * Lav grub config (BIOS/Legacy boot)
    `sudo grub2-mkconfig > /boot/grub2/grub.cfg`

    * Installer **dnf-plugins-core** - pakken
    `sudo dnf -y install dnf-plugins-core`

    * Opsæt brug af **stable** repository
    `sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo`

    * Installer den seneste version af Docker Engine
    `sudo dnf install docker-ce docker-ce-cli containerd.io`

    * (Optional) Installer **docker-compose**
    `sudo dnf install docker-compose`

    * Tilføj brugeren til docker gruppen
    `sudo usermod -aG docker $(whoami)`
    Eller hvis det er en anden specifik bruger der skal bruges til docker
    `sudo usermod -aG docker USERNAME`
    Bemærk at det er bedst at have en bruger i docker gruppen, da man så ikke skal køre alt docker relateret med root.

    * Sæt docker servicen til at starte med operativsystemet
    `sudo systemctl enable docker`
    Man kan allerede også starte docker med
    `sudo systemctl start docker`
    Bemærk dog at du skal have genstartet for at cgroups er ændret til 1 inden docker fungere.

    * Genstart maskinen
    `reboot`

    * Herefter kan man tjekke om docker virker
    `sudo docker run hello-world`

3. **Installer kubectl**

    * Download kubectl
    ```curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl```

    * Gør kubectl eksekverbar
    `chmod +x ./kubectl`

    * Flyt kubectl så den kan bruges som en service
    `sudo mv ./kubectl /usr/local/bin/kubectl`

4. **Installer Minikube**

    * Installer Virtualization gruppen af pakker
    `sudo dnf group install Virtualization`

    * Tilføj brugeren til gruppe, så vi igen ikke har behov for root
    `sudo usermod -aG libvirt $(whoami) && newgrp libvirt`
    Denne kommando opretter også en nye gruppe der hedde **libvirt** i tilfælde af den ikke eksistere.

    * Download Minikube
    `curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64`

    * Gør minikube eksekverbar
    `chmod +x minikube`

    * Installer minikube så den kan bruges som service
    `sudo install minikube /usr/local/bin/`

5. **Kør Minikube**
    Der er flere måder / flere vm-drivers man kan bruge til at køre Minikube i.

    * Kør Minikube (Podman)
    Podman er installeret på Fedora som standard og burde stadig være til stede, da denne guide ikke fjerner tidligere Docker versioner, som også fjerner cmon-servicen som Podman er afhængig af og bliver derfor også afinstalleret.
    Minikube kan startes med Podman (Kræver root rettigheder)
    `sudo minikube start --vm-driver podman`

    * Kør Minikube (Docker)
    `minikube start --vm-driver docker`
    Bemærk der ikke kræves root rettigheder.

    * Kør Minikube (Bare Metal / Direkte på maskinens OS)
    Tilføj port-rækkevidden [8443 10250] til firewallen, da disse porte bliver brugt til kommunikationen udadtil
    `firewall-cmd --add-port=8443-10250/tcp`

        For at tilføje firewall reglen permanent, så den er aktiv efter genstart
        `firewall-cmd --add-port=8443-10250/tcp --permanent`

        Deaktiver SELinux for at den ikke blockere starten af minikube
        `sudo setenforce 0`
        Dette deaktivere kun for den nuværende session og SELinux køre igen efter genstart.

        For at deaktivere SELinux permanent skal man redigere **/etc/sysconfig/selinux** - filen. Dette kan gøres med en text-editor som f.eks. nano
        `nano /etc/sysconfig/selinux`
        Hvor man ændre linjen **SELINUX=enforcing** til **SELinux=disabled**
        
        Herefter genstarter man med **reboot**

        Brug **sestatus** for at tjekke om SELinux er slået fra. Kommandoen skulle gerne vise **Permissive** under **Current mode** eller **Disabled**, hvis SELinux permanent er deaktiveret.

        Minikube kan herefter køres (Kræver root rettigheder)
        `sudo minikube start --vm-driver none`

6. Konfigurering af Minikube (Optional)
Minikube har en række standard værdier man kan sætte for gøre start kommandoen mere simpel.

    Man kan sætte vm-driveren standard til f.eks. Docker
    `minikube config set vm-driver docker`
    På den måde slipper man for at skrive **--vm-driver docker** hver gang minikube skal startes.

    Samtidig kan man også sætte mængden af hukkommelse der bliver tildelt minikube med
    `minikube config set momory 1024`
    Dette brugte jeg personligt en del til at begrænse resourcerne den tog på min bæbare computer.

    For at tjekke en specifik config værdi som f.eks. vm-driveren kan man skrive
    `minikube config get vm-driver`

    For at se alle konfigurerbar værdier kan man bruge
    `minikube config -h`

    **NOTE:** Hvis man skriver **sudo** foran alle config kommandoerne, så bliver de sat for root brugeren og ikke den bruger der skriver kommandoen. Det er derfor vigtigt, at huske om man bruger en **vm-driver** der kræver root rettigheder eller ej, da man så skal huske at sætte værdierne for den rettige bruger.


**Kilder**
* <https://medium.com/learning-cloud-native-go/setting-up-a-fedora-31-workstation-for-go-cloud-native-application-development-77700467bf7b>

* <https://docs.docker.com/install/linux/docker-ce/fedora/>

* <https://askubuntu.com/questions/162564/how-can-i-tell-if-my-system-was-booted-as-efi-uefi-or-bios>

* <https://www.tecmint.com/disable-selinux-in-centos-rhel-fedora>
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

<h3>Installing Minikube on Fedora</h3>

1. <b>Opdater Fedora</b>
    * `sudo dnf update`

2. <b>Installer Docker</b>

    * Ændre cgroups til 1 (Første metode)
        * Installer din foretrukne text-editor (Jeg bruger nano her)<br>
        `sudo dnf install nano`

        * Rediger grub filen<br>
        `sudo nano /etc/default/grub`

        * Tilføj `systemd.unified_cgroup_hierarchy=0` til enden af `GRUB_CMDLINE_LINUX="..."` linjen.

        * Din grub fil skulle så gerne have en linje der ligner denne<br>
        `GRUB_CMDLINE_LINUX="resume=UUID=... rhgb quiet systemd.unified_cgroup_hierarchy=0"`
    
    * Ændre cgroups til 1 (Anden metode)

        * Installer grubby<br>
        `sudo dnf install grubby`

        * Ændre cgroups med<br>
        `sudo grubby --update-kernel=ALL --args="systemd.unified_cgroup_hierarchy=0"`<br><br>
        <b>OBS:</b> Anden metode med grubby afspejler sig nødvendigvis ikke i selve `/etc/default/grub` - filen og jeg har erfaret at den ikke altid træder i kraft efter første genstart. Her kan man tjekke ens maskine med `virt-host-validate` for at se om nogle punkter fejler. Hvis der er nogle punkter der er røde og siger "failed", så prøv at køre kommandoen igen og genstart. Umiddelbart ligner er der ingen problemer efter man har kørt kommandoen anden gang og genstartet, dog er årsagen til fejlen ikke kendt.

    * Tjek om maskinen bruger EFI/UEFI eller BIOS/Legacy boot med bash<br>
    `[ -d /sys/firmware/efi ] && echo UEFI || echo BIOS`<br>
    Denne kommando / lille script tjekker om `/sys/firmware/efi` - mappen eksitere. Hvis den gør så har maskinen startet med EFI/UEFI boot, hvis ikke er den startet med BIOS boot.
    
    * Lav grub config (EFI/UEFI boot)<br>
    `sudo grub2-mkconfig > /boot/efi/EFI/fedora/grub.cfg`

    * Lav grub config (BIOS/Legacy boot)<br>
    `sudo grub2-mkconfig > /boot/grub2/grub.cfg`

    * Installer `dnf-plugins-core` - pakken<br>
    `sudo dnf -y install dnf-plugins-core`

    * Opsæt brug af <b>stable</b> repository<br>
    `sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo`

    * Installer den seneste version af Docker Engine<br>
    `sudo dnf install docker-ce docker-ce-cli containerd.io`

    * (Optional) Installer `docker-compose`<br>
    `sudo dnf install docker-compose`

    * Tilføj brugeren til docker gruppen<br>
    `sudo usermod -aG docker $(whoami)`<br>
    Eller hvis det er en anden specifik bruger der skal bruges til docker<br>
    `sudo usermod -aG docker USERNAME`<br>
    Bemærk at det er bedst at have en bruger i docker gruppen, da man så ikke skal køre alt docker relateret med root.

    * Sæt docker servicen til at starte med operativsystemet<br>
    `sudo systemctl enable docker`<br>
    Man kan allerede også starte docker med<br>
    `sudo systemctl start docker`<br>
    Bemærk dog at du skal have genstartet for at cgroups er ændret til 1 inden docker fungere.

    * Genstart maskinen<br>
    `reboot`

    * Herefter kan man tjekke om docker virker<br>
    `sudo docker run hello-world`

3. <b>Installer kubectl</b>

    * Download kubectl<br>
    ```curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl```

    * Gør kubectl eksekverbar<br>
    `chmod +x ./kubectl`

    * Flyt kubectl så den kan bruges som en service<br>
    `sudo mv ./kubectl /usr/local/bin/kubectl`

4. <b>Installer Minikube</b>

    * Installer Virtualization gruppen af pakker<br>
    `sudo dnf group install Virtualization`

    * Tilføj brugeren til gruppe, så vi igen ikke har behov for root<br>
    `sudo usermod -aG libvirt $(whoami) && newgrp libvirt`<br>
    Denne kommando opretter også en nye gruppe der hedde `libvirt` i tilfælde af den ikke eksistere.

    * Download Minikube<br>
    `curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64`

    * Gør minikube eksekverbar<br>
    `chmod +x minikube`

    * Installer minikube så den kan bruges som service<br>
    `sudo install minikube /usr/local/bin/`

5. <b>Kør Minikube</b><br>
    Der er flere måder / flere vm-drivers man kan bruge til at køre Minikube i.

    * Kør Minikube (Podman)<br>
    Podman er installeret på Fedora som standard og burde stadig være til stede, da denne guide ikke fjerner tidligere Docker versioner, som også fjerner cmon-servicen som Podman er afhængig af og bliver derfor også afinstalleret.<br><br>
    Minikube kan startes med Podman (Kræver root rettigheder)<br>
    `sudo minikube start --vm-driver podman`

    * Kør Minikube (Docker)<br>
    `minikube start --vm-driver docker`<br>
    Bemærk der ikke kræves root rettigheder.

    * Kør Minikube (Bare Metal / Direkte på maskinens OS)<br>
    Tilføj port-rækkevidden [8443 10250] til firewallen, da disse porte bliver brugt til kommunikationen udadtil<br>
    `firewall-cmd --add-port=8443-10250/tcp`<br><br>
    For at tilføje firewall reglen permanent, så den er aktiv efter genstart<br>
    `firewall-cmd --add-port=8443-10250/tcp --permanent`<br><br>
    Deaktiver SELinux for at den ikke blockere starten af minikube<br>
    `sudo setenforce 0`<br>
    Dette deaktivere kun for den nuværende session og SELinux køre igen efter genstart.<br><br>
    For at deaktivere SELinux permanent skal man redigere `/etc/sysconfig/selinux` - filen. Dette kan gøres med en text-editor som f.eks. nano<br>
    `nano /etc/sysconfig/selinux`<br>
    Hvor man ændre linjen `SELINUX=enforcing` til `SELinux=disabled`<br>
    Herefter genstarter man med `reboot`<br><br>
    Brug `sestatus` for at tjekke om SELinux er slået fra. Kommandoen skulle gerne vise `Permissive` under `Current mode` eller `Disabled`, hvis SELinux permanent er deaktiveret.<br><br>
    Minikube kan herefter køres (Kræver root rettigheder)<br>
    `sudo minikube start --vm-driver none`

6. Konfigurering af Minikube (Optional)<br>
Minikube har en række standard værdier man kan sætte for gøre start kommandoen mere simpel.<br><br>
Man kan sætte vm-driveren standard til f.eks. Docker<br>
`minikube config set vm-driver docker`<br>
På den måde slipper man for at skrive `--vm-driver docker`, hver gang minikube skal startes.

<b>Kilder</b>
* https://medium.com/learning-cloud-native-go/setting-up-a-fedora-31-workstation-for-go-cloud-native-application-development-77700467bf7b

* https://docs.docker.com/install/linux/docker-ce/fedora/

* https://askubuntu.com/questions/162564/how-can-i-tell-if-my-system-was-booted-as-efi-uefi-or-bios

* https://www.tecmint.com/disable-selinux-in-centos-rhel-fedora/
Arch Linux
==========

_Versão 1 - atualizada em 05/05/2017_

-----

## Instlação Básica:

Mudar o layout do eclado para abnt2:

```bash
$ loadkeys br-abnt2
```

Configurar o idioma de intalação:

```bash
$ vim /etc/locale.gen
```

descomentar essas linhas:

```
en_US.UTF-8 UTF-8
pt_BR.UTF-8 UTF-8
```

```bash
$ locale-gen
$ export LANG=pt_BR.UTF-8
```

Testar conexão com a internet:

```bash
$ ping -c 3 www.google.com
```

Particionar o disco:

```bash
$ fdisk -l
$ cfdisk /dev/sda
```

Tamannhos das partições:

- 2MB - inicialização
- 38GB - /
- 2GB - swap

Formatar as particoes:

```bash
$ mkfs.ext4 /dev/sda2
$ mkswap /dev/sda3
$ swapon /dev/sda3
$ lsblk /dev/sda
```

Montar as partições:

```bash
$ mount /dev/sda2 /mnt
```
Ajustar o mirrorlist:

```bash
$ vim /etc/pacman.d/mirrorlist
```

Colocar os espelhos do brasil no inicio do arquivo.

Instalar o sistema base:

```bash
$ pacstrap /mnt base grub
```

Gerar o arquivo fstab que decreve as particoes:

```bash
$ genfstab -p /mnt >> /mnt/etc/fstab
$ cat /mnt/etc/fstab
```

Logar na instalação:

```bash
$ arch-chroot /mnt
```

Setar o hostname:

```bash
$ vi /etc/hostname
```

colocar "ArchLinuxVM-01"

```
ArchLinuxVM-01
```

Configurar o fuso horário:

```bash
$ ls /usr/share/zoneinfo/America
$ ln -sf /usr/share/zoneinfo/America/Recife /etc/localtime
```

Configurar o idioma:

```bash
$ vi /etc/locale.gen
```

descomentar as linhas:

```ig
en_US.UTF-8 UTF-8
en_US.ISO-8859-1
pt_BR.UTF-8 UTF-8
pt_BR.ISO-8859-1
```

```bash
$ locale-gen
$ echo LANG=pt_BR.UTF-8 >> /etc/locale.conf
$ export LANG=pt_BR.UTF-8
```

Configurar o layout do teclado:

```bash
$ vi /etc/vconsole.conf
```

```
KEYMAP=br-abnt2
FONT=Lat2-Terminus16
FONT_MAP=
```

Configurar a inicialização:

```bash
$ mkinitcpio -p linux
```

Configurar a rede cabeada:

```bash
_$ systemctl enable dhcpcd@eth0.service_
```

Criar senha de root:

```bash
$ passwd
```

Intalando o Grub

```bash
$ pacman -S grub-bios
$ grub-install --target=i386-pc --recheck /dev/sda
$ cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo
$ grub-mkconfig -o /boot/grub/grub.cfg
```

_$ arch-chroot /mnt pacman -S grub-bios_

Sair do chroot:

```bash
$ exit
```

Desmontar as particoes:

```bash
$ umount /mnt
```

Reiniciar

```bash
$ reboot
```
Configuração do ArchLinux
=========================

### Atulizar o sistema

Ajustes nos repositórios

```bash
$ vim /etc/pacman.conf
```

descomentar multilib

```bash
$ pacman -Syu
```

### Instalar o vim (pelo amor de Deus)

```bash
$ pacman -S vim
```

### Criar usuario e definir senha

```bash
$ useradd -m -g users -G wheel,storage,power -s /bin/bash usuario
$ passwd usuario
```

### Instalar sudo

```bash
$ pacman -S sudo
```

Editar as permissões:

```bash
$ EDITOR=vim visudo
```

Descomentar a linha que mostra o grupo (wheel).

### Configurar o som

```bash
$ pacman -S alsa-utils
$ alsamixer
```

### Driver virtualbox:

```bash
$ sudo pacman -S virtualbox-guest-utils
```

### Instalar xorg

```bash
$ sudo pacman -S xorg-server
$ sudo pacman -S xorg-xinit
$ sudo pacman -S xorg-server-utils
```

### Instalar fonts

```bash
$ sudo pacman -S ttf-dejavu
```

### Gerenciador de rede

```bash
$ sudo pacman -S networkmanager

# NÃO PRECISA - MAS PRECISO CHECAR
$ sudo pacman -S networkmanager-vpnc
$ sudo pacman -S networkmanager-pptp
$ sudo pacman -S networkmanager-openconnect
```

```bash
$ sudo pacman -S network-manager-applet
```

Ativar o gerenciador de rede na inicialização do sistema:

```bash
$ sudo systemctl enable NetworkManager
```

### ssh

```
$ sudo pacman -S openssh
```

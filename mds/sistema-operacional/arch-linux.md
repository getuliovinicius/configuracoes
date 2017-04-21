Arch Linux
==========

## Instlação Básica:

Mudar o layout do eclado para abnt2:

``` sh
$ loadkeys br-abnt2
```

Configurar o idioma de intalação:

``` sh
$ vim /etc/locale.gen
```

descomentar essas linhas:

``` conf
en_US.UTF-8 UTF-8
pt_BR.UTF-8 UTF-8
```

``` sh
$ locale-gen
$ export LANG=pt_BR.UTF-8
```

Testar conexão com a internet:

``` sh
$ ping -c 3 www.google.com
```

Particionar o disco:

``` sh
$ fdisk -l
$ cfdisk /dev/sda
```

Tamannhos das partições:

- 2MB - inicialização
- 38GB - /
- 2GB - swap

Formatar as particoes:

``` sh
$ mkfs.ext4 /dev/sda2
$ mkswap /dev/sda3
$ swapon /dev/sda3
$ lsblk /dev/sda
```

Montar as partições:

``` sh
$ mount /dev/sda2 /mnt
```
Ajustar o mirrorlist:

``` sh
$ vim /etc/pacman.d/mirrorlist
```

Colocar os espelhos do brasil no inicio do arquivo.

Instalar o sistema base:

``` sh
$ pacstrap /mnt base grub
```

Gerar o arquivo fstab que decreve as particoes:

``` sh
$ genfstab -p /mnt >> /mnt/etc/fstab
$ cat /mnt/etc/fstab
```

Logar na instalação:

``` sh
$ arch-chroot /mnt
```

Setar o hostname:

``` sh
$ vi /etc/hostname
```

colocar "ArchLinuxVM-01"

``` conf
ArchLinuxVM-01
```

Configurar o fuso horário:

``` sh
$ ls /usr/share/zoneinfo/America
$ ln -sf /usr/share/zoneinfo/America/Recife /etc/localtime
```

Configurar o idioma:

``` sh
$ vi /etc/locale.gen
```

descomentar as linhas:

``` config
en_US.UTF-8 UTF-8
en_US.ISO-8859-1
pt_BR.UTF-8 UTF-8
pt_BR.ISO-8859-1
```

``` sh
$ locale-gen
$ echo LANG=pt_BR.UTF-8 >> /etc/locale.conf
$ export LANG=pt_BR.UTF-8
```

Configurar o layout do teclado:

``` sh
$ vi /etc/vconsole.conf
```

``` conf
KEYMAP=br-abnt2
FONT=Lat2-Terminus16
FONT_MAP=
```

Configurar a inicialização:

``` sh
$ mkinitcpio -p linux
```

Configurar a rede cabeada:

``` sh
_$ systemctl enable dhcpcd@eth0.service_
```

Criar senha de root:

``` sh
$ passwd
```

Intalando o Grub

``` sh
$ pacman -S grub-bios
$ grub-install --target=i386-pc --recheck /dev/sda
$ cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo
$ grub-mkconfig -o /boot/grub/grub.cfg
```
_$ arch-chroot /mnt pacman -S grub-bios_

Sair do chroot:

``` sh
$ exit
```

Desmontar as particoes:

``` sh
$ umount /mnt
```

Reiniciar

``` sh
$ reboot
```
Configuração do ArchLinux
=========================

### Atulizar o sistema

Ajustes nos repositórios

``` sh
$ vim /etc/pacman.conf
```

descomentar multilib

``` sh
$ pacman -Syu
```

### Instalar o vim (pelo amor de Deus)

``` sh
$ pacman -S vim
```

### Criar usuario e definir senha

``` sh
$ useradd -m -g users -G wheel,storage,power -s /bin/bash usuario
$ passwd usuario
```

### Instalar sudo

``` sh
$ pacman -S sudo
```

Editar as permissões:

``` sh
$ EDITOR=vim visudo
```

Descomentar a linha que mostra o grupo (wheel).

### Configurar o som

``` sh
$ pacman -S alsa-utils
$ alsamixer
```

### Driver virtualbox:

``` sh
$ sudo pacman -S virtualbox-guest-utils
```

### Instalar xorg

``` sh
$ sudo pacman -S xorg-server
$ sudo pacman -S xorg-xinit
$ sudo pacman -S xorg-server-utils
```

### Instalar fonts

``` sh
$ sudo pacman -S ttf-dejavu
```

### Gerenciador de rede

``` sh
$ sudo pacman -S networkmanager

# NÃO PRECISA - MAS PRECISO CHECAR
_$ sudo pacman -S networkmanager-vpnc
$ sudo pacman -S networkmanager-pptp
$ sudo pacman -S networkmanager-openconnect_
```

``` sh
$ sudo pacman -S network-manager-applet
```

Ativar o gerenciador de rede na inicialização do sistema:

``` sh
$ sudo systemctl enable NetworkManager
```

### ssh

```
$ sudo pacman -S openssh
```

### Arquivo “/etc/hosts”

Para o ambiente de desenvolvimento pretendido é preciso atribuir uma entrada no arquivo `/etc/hosts`, para cada domínio local que se deseja acessar.
Essa ação fará com que os navegadores web instalados no computador possam acessar páginas locais, servidas no próprio computador, a partir de domínios locais.
No exemplo abaixo existe uma entrada para o domínio local `app1.local`, que pode ser acessado no navegador com o endereço “http://app1.local”.
Mostar o conteúdo do arquivo `/etc/hosts`:

``` sh
$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	ARCH-LINUX-PC # este nome é definido na instalação
127.0.0.1	app1.local # exemplo de entrada para um projeto
```

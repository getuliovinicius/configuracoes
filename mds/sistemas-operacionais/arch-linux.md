Arch Linux
==========

_Versão 1 - atualizada em 09/07/2017_

-----

# Instalação Básica

+ Rodar o boot pela imagem ".iso" baixada.
+ Mudar o layout do teclado para **abnt2**:

```bash
$ loadkeys br-abnt2
```

+ Testar a conexão com a internet com o comando `ping`:

```bash
$ ping -c 3 google.com
```

+ Atualizar a base de dados de pacotes:

```bash
$ pacman -Syy
```

+ Listar os discos e as partições:

```bash
$ fdisk -l
```

+ Particionar o disco:

```bash
$ cfdisk /dev/sda
```

+ Escolher o modelo de tabela de discos **dos** e configurar os Tamanhos das partições:

```
36GB - no inicio para a raiz do sistema "/"
4GB - para swap
```

+ Formatar as partições:

```bash
$ mkfs.ext4 /dev/sda1
$ mkswap /dev/sda2
$ swapon /dev/sda2
```

+ Montar as partições:

```bash
$ mount /dev/sda1 /mnt
$ lsblk
```

+ Instalar o sistema base:

```bash
$ pacstrap -i /mnt base base-devel
```

+ Gerar o arquivo `/etc/fstab` que descreve as partições:

```bash
$ genfstab -U -p /mnt >> /mnt/etc/fstab
$ cat /mnt/etc/fstab
```

+ Fazer login na instalação e chamar o `bash` para interagir na linha de comando:

```bash
$ arch-chroot /mnt /bin/bash
```

+ Configurar o idioma de instalação editando o arquivo `/etc/locale.gen`:

```bash
$ vi /etc/locale.gen
```

+ Descomentar as seguintes linhas no arquivo:

```
en_US.UTF-8 UTF-8
en_US.ISO-8859-1
pt_BR.UTF-8 UTF-8
pt_BR.ISO-8859-1
```

+ Persistir a alteração do idioma:

```bash
$ locale-gen
$ echo LANG=pt_BR.UTF-8 >> /etc/locale.conf
$ export LANG=pt_BR.UTF-8
```

+ Configurar o layout do teclado:

```bash
$ vi /etc/vconsole.conf
```

```
KEYMAP=br-abnt2
FONT=Lat2-Terminus16
FONT_MAP=
```

+ Configurar o fuso horário:

```bash
$ ls /usr/share/zoneinfo/America
$ ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime
# não rode o comando abaixo pois ainda não entendo o pq ele não funciona
$ hwclock --systohc --local
```

+ Setar o `hostname`:

```bash
$ echo ArchLinuxVM-modelo >> /etc/hostname
```

+ Editar o arquivo `/etc/hosts`:

```bash
$ vi /etc/hosts
```

+ Adicionar a linha abaixo na sessão apropriada do arquivo, antes da linha `# End of file`:

```
127.0.1.1   ArchLinuxVM-modelo.localdomain  ArchLinuxVM-modelo
```

+ Ativar a busca por IP automático, DHCP, na inicialisação do sistema:

```bash
$ systemctl enable dhcpcd.service
```

+ Criar senha de root:

```bash
$ passwd
```

123456

+ Instalar o Grub

```bash
$ pacman -S grub
$ grub-install /dev/sda
$ grub-mkconfig -o /boot/grub/grub.cfg
```

+ Fazer logout na instalação:

```bash
$ exit
```

+ Desmontar as partições:

```bash
$ umount -R /mnt
```

+ Reiniciar

```bash
$ reboot
```

# Configuração do ArchLinux

+ Logar como root
+ Atualizar o ArchLinux:

```bash
$ pacman -Syu
```

+ Instalar o pacote `reflector` que recupera a lista de espelhos de rede mais rápidos:

```bash
$ pacman -S reflector
```

+ Copiar a lista de espelhos para um backup:

```bash
$ cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.original
```

+ Localizar e salvar os melhores espelhos de rede no arquivo `/etc/pacman.d/mirrorlist`:

```bash
$ reflector -c Brazil --save /etc/pacman.d/mirrorlist
$ cat /etc/pacman.d/mirrorlist
```

!!! note "Nota"
    Se o `reflector` não funcionar, não reconhecia urls do tipo `https`, tente instalar o o `openssl`, comando abaixo, e depois tente o comando para salvar os espelhos de rede.

+ Instalar o `openssl`:

```bash
$ pacman -S openssl
```

+ Instalar o vim **(pelo amor de Deus)**:

```bash
$ pacman -S vim
```

+ Instalar o `sudo`:

```bash
$ pacman -S sudo
```

+ Criar usuário comum e definir senha:

```bash
$ useradd -m -g users -G wheel -s /bin/bash usuario
$ passwd usuario
```

+ Editar as permissões:

```bash
$ EDITOR=vim visudo
```

+ Descomentar a linha que mostra o grupo (wheel).

```
%wheel ALL=(ALL) ALL
```

+ Fazer logout do usuário root:

```bash
$ exit
```

+ Logar com o novo usuário:
+ Instalar os drivers de audio:

```bash
$ pacman -S pulseaudio pulseaudio-alsa
```

# Servidor X

+ Instalar o servidor de interface gráfica `xorg`:

```bash
$ sudo pacman -S xorg xorg-xinit
```

+ Driver gráfico do Virtualbox:

```bash
$ sudo pacman -S virtualbox-guest-utils
```

!!! note "Nota"
    A instalação do pacote `virtualbox-guest-utils` deve ser feita apenas se a instalação foi realizada em Máquina Virtual com o Virtualbox.

# Instalação da interface gráfica KDE

+ Configurar o arquivo `~/.xinitrc`:

```bash
$ echo "exec startkde" > ~/.xinitrc
```

+ Instalar o KDE mínimo:

```bash
$ sudo pacman -S plasma-desktop
```

+ Instalar alguns programas básicos:

```bash
$ sudo pacman -S konsole dolphin firefox kate
```

+ Iniciar a interface gráfica:

```bash
$ startx
```

+ Abrir um terminal gráfico, instalar o gerenciador de sessão e ativá-lo na inicialização do sistema:

```bash
$ sudo pacman -S sddm
$ sudo systemctl enable sddm.service
```

!!! warning "Nota"
    Os passos abaixo ainda não foram testados.

+ Configurar...:

```bash
$ mkinitcpio -p linux
```

+ Instalar fonts:

```bash
$ sudo pacman -S ttf-dejavu
```

+ Instalar gerenciador de rede:

```bash
$ sudo pacman -S networkmanager
```

```bash
$ sudo pacman -S network-manager-applet
```

+ Ativar o gerenciador de rede na inicialização do sistema:

```bash
$ sudo systemctl enable NetworkManager
```

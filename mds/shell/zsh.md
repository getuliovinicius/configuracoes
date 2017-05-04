zsh
===

Comando para instalar o _zsh_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

```bash
$ sudo pacman -S zsh
```

Comando para instalar o _zsh_ nos sistemas operacionais _Debian, Ubuntu, Linux Mint_:

```bash
$ sudo apt install zsh
```

Comandos para tornar o _zsh_ o shell padrão para o seu usuário:

```bash
$ sudo usermod -s /bin/zsh seu_usuario
```

ou

```bash
$ chsh -s /bin/zsh
```

## _oh-my-zsh_

O _[oh-my-zsh](http://ohmyz.sh/)_ é um _framework_ que facilita a configuração do _shell_ _ZSH_.

Comandos para instalar o _oh-my-zsh_ em qualquer sistema operacional _Linux_:

+ Framework

```bash
$ sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

+ Fontes com icones para o terminal (opicional)

```bash
$ git clone https://github.com/powerline/fonts.git
$ cd fonts
$ ./install.sh
$ cd ..
$ rm -rf fonts
```
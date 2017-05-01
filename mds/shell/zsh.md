zsh
===

Comando para instalar o _zsh_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

``` sh
$ sudo pacman -S zsh
```

Comando para instalar o _zsh_ nos sistemas operacionais _Debian, Ubuntu, Linux Mint_:

``` sh
$ sudo apt install zsh
```

Comandos para tornar o _zsh_ o shell padrão para o seu usuário:

``` sh
$ sudo usermod -s /bin/zsh seu_usuario

# ou

$ chsh -s /bin/zsh
```

## _oh-my-zsh_

O _[oh-my-zsh](http://ohmyz.sh/)_ é um _framework_ que facilita a configuração do _shell_ _ZSH_.

Comandos para instalar o _oh-my-zsh_ em qualquer sistema operacional _Linux_:

``` sh
# framework
$ sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

# fonts com icones para o terminal (opicional)
$ git clone https://github.com/powerline/fonts.git
$ cd fonts
$ ./install.sh
$ cd ..
$ rm -rf fonts
```
<<<<<<< HEAD:mds/shell/oh-my-zsh.md

Comandos para tornar o _oh-my-zsh_ o shell padrão para o seu usuário:

``` sh
$ sudo usermod -s /bin/zsh seu_usuario

# ou

$ chsh -s /bin/zsh
```
=======
>>>>>>> cfec41cf68195d6163f124d81e5575f673a4641c:mds/shell/zsh.md

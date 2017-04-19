oh-my-zsh
=========

Comando para instalar o _zsh_ nos sistemas operacionais _Debian, Ubuntu, Linux Mint_:

``` sh
$ sudo apt install zsh
```

Comandos para instalar o _oh-my-zsh_ em qualquer sistema operacional _Linux_:

``` sh
$ sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

# fonts com icones para o terminal (opicional)
$ git clone https://github.com/powerline/fonts.git
$ cd fonts
$ ./install.sh
$ cd ..
$ rm -rf fonts
```

Comandos para tornar o _oh-my-zsh_ o shell padrão para o seu usuário:

``` sh
$ sudo usermod -s /bin/zsh seu_usuario

# ou

$ chsh -s /bin/zsh
```
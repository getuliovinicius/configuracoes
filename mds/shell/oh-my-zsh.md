zsh
===

Comando para instalar o _zsh_ nos sistemas operacionais _Debian, Ubuntu, Linux Mint_:

``` sh
$ sudo apt install zsh
```

Comandos para instalar o _oh-my-zsh_ em qualquer sistema operacional _Linux_:

``` sh
$ sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

# clone
$ git clone https://github.com/powerline/fonts.git

# install
$ cd fonts
$ ./install.sh

# clean-up a bit
$ cd ..
$ rm -rf fonts
```

``` sh
$ sudo usermod -s /bin/zsh seu_usuario
$ chsh -s /bin/zsh
```
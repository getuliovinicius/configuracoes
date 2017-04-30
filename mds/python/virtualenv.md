Ambiente Virtual _Python_
=========================

Este é um roteiro para configuração de um ambiente virtual _Python_, de modo que o trabalho e o Sistema Operacional se mantenham organizados enquanto as bibliotécas de código, os _frameworks_, etc vão sendo instalados, assim como os projetos.

## pip

O _pip_ é um gerenciador de pacotes e dependências usado para instalar pacotes de software escritos em _Python_ a partir de um grande repositório da comunidade: [https://pypi.python.org/pypi](https://pypi.python.org/pypi).

Comando para instalar o _pip_ em qualquer sistema operacional _Linux_:

``` sh
wget https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
```

Comando para instalar o _pip_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

``` sh
$ sudo pacman -S python-pip
```

## virtualenv

O _virtualenv_ é uma ferramenta que permite diferentes ambientes virtuais de _Python_ na mesma máquina. Isso facilita a organização do sistema operacional - SO, pois com o _virtualenv_ não é preciso manter um pacote instalado globalmente no _SO_ sendo que o mesmo só é útil para um dos projetos que eventualmente está se desenvolvendo, essa prática evita conflitos de dependências.

Comando para instalar o _virtualenv_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

``` sh
$ sudo pip install virtualenv
```

### virtualenvwrapper

0 _virtualenvwrapper_ é um conjunto de extensões para a ferramenta _virtualenv_. As extensões incluem comandos para criar e excluir ambientes virtuais, tornando mais fácil gerenciar e trabalhar em mais de um projeto no computador sem introduzir conflitos em suas dependências.

Comando para instalar o _virtualenvwrapper_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

``` sh
$ sudo pip install virtualenvwrapper
```

Para concluir a instalação é preciso editar o arquivo `~/.bashrc` ou `~/.zshrc`, depende do _shell_ que está definido como padrão no _SO_.

Pode-se usar o editor _vim_ para fazer a edição:

``` sh
$ vim ~/.bashrc

# ou

$ vim ~/.zshrc
```

As linhas abaixo devem ser adicionadas no final do arquivo:

``` .conf
# Para Arch Linux e Manjaro Linux utilize as linhas abaixo:

# Virtualenv
export WORKON_HOME=$HOME/Virtualenvs
export PROJECT_HOME=$HOME/Devel
source /usr/bin/virtualenvwrapper.sh

# Para _Debian, Ubuntu, Linux Mint_ utilize as linhas abaixo:

# Virtualenvwrapper
export WORKON_HOME=~/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
export PIP_REQUIRE_VIRTUALENV=true
```

É preciso recarregar as configurações do _bash_ ou _zsh_ com o comando:

``` sh
$ source ~/.bashrc

# ou

$ source ~/.zshrc
```

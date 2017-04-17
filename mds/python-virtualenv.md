Ambiente Virtual Python
=======================

Abaixo segue o roteiro utilizado para instalar o Ambiente Python necessário para utilização da ferramenta de criação de documentação _Mkdocs_.

## pip

O _pip_ é um sistema de gerenciamento de pacotes usado para instalar e gerenciar pacotes de software escritos na linguagem de programação Python.

Comando para instalar o _pip_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

``` sh
$ sudo pacman -S python-pip
```

Referência: [Devfuria](http://www.devfuria.com.br/linux/instalando-pip/)

## virtualenv

O _virtualenv_ é uma ferramenta que permite diferentes ambientes virtuais de Python na mesma maquina.

Comando para instalar o _virtualenv_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

``` sh
$ sudo pip install virtualenv
```

### virtualenvwrapper

0 _virtualenvwrapper_ é um conjunto de extensões para a ferramenta _virtualenv_. As extensões incluem _wrappers_ para criar e excluir ambientes virtuais e gerenciar seu fluxo de trabalho de desenvolvimento, tornando mais fácil trabalhar em mais de um projeto de cada vez sem introduzir conflitos em suas dependências. - [fonte](https://virtualenvwrapper.readthedocs.io/en/latest/index.html)

Comando para instalar o _virtualenvwrapper_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

``` sh
$ sudo pip install virtualenvwrapper
```

Para concluir a instalação é preciso editar o arquivo `~/.bashrc`. Pode-se usar o editor _vim_:

``` sh
$ vim ~/.bashrc
```

As linhas abaixo devem ser adicionadas no final do arquivo:

``` .conf
# Virtualenv
export WORKON_HOME=$HOME/Virtualenvs
export PROJECT_HOME=$HOME/Devel
source /usr/bin/virtualenvwrapper.sh
```

É preciso recarregar as configurações do _bash_ com o comando:

``` sh
$ source ~/.bashrc
```

O que é o Git?
==============

O “Git” é um sistema de versionamento de aquivos, projetos, etc. A _playlist_ do canal **RB Tech** no _YouTube_ é extremamente recomendada para compreensão do Git.

- [Playlist no YouTube](https://youtu.be/WVLhm1AMeYE?list=PLInBAd9OZCzzHBJjLFZzRl6DgUmOeG3H0)

## Instalação

Comando para instalar o _git_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

``` sh
$ sudo pacman -S git
```

Comando para instalar o _git_ nos sistemas operacionais _Debian, Ubuntu, Linux Mint_:

``` sh
$ sudo apt install git
```

## Configuração

Comandos para configurar as opções globais do _git_:

``` sh
$ git config --global user.name "Seu Nome"
$ git config --global user.email "seunome@exemplo.com"
$ git config --list
```

## Bitbucket e GitHub

O _Bitbucket e o GitHub_ são serviços para hospedagem de repositórios _git_ e também uma espécie de rede social de programadores na qual podemos disponibilizar projetos, receber contribuições e também contribuir com outros projetos.
Basta criar uma conta no serviço que desejar e depois adicionar as chaves _ssh_ do computador no perfil.
Links para os serviços:

- [bitbucket.org](http://bitbucket.org)
- [github.com](http://github.com)

### xclip

Para facilitar a cópia da chave _ssh_ pode-se usar o _xclip_, que copia textos para área de transferência.

Comando para instalar o _xclip_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

``` sh
$ sudo pacman -S xclip
```

Comando para instalar o _xclip_ nos sistemas operacionais _Debian, Ubuntu, Linux Mint_:

``` sh
$ sudo apt install xclip
```

Com o _xclip_ instalado e com a conta configurada, ou no _Bitbucket_ ou no _GitHub_, pode-se gerar as chaves _ssh_ com os seguinte comando:

``` sh
$ ssh-keygen -t rsa -b 4096 -C "seunome@exemplo.com"
```

Comando para verificar a criação da chave _ssh_:

``` sh
$ ls -la .ssh/
```

Comando para copiar a chave _ssh_ para a área de transferência:

``` sh
$ xclip -sel clip < ~/.ssh/id_rsa.pub
```

Após adicionar a chave _ssh_ no _Bitbucket_ ou no _GitHub_, pode-se testar a conexão com o comando:

``` sh
# Bitbucket
$ ssh -vT git@bitbucket.org

# GitHub
$ ssh -vT git@github.com
```

Comando para iniciar um repositório:

``` sh
$ git init
```

Comando para relacionar um repositório local a um repositório criado em um dos serviços de hospedagem de repositório _git_:

``` sh
$ git remote add origin https://github.com/xxxxx/xxxxx.git
```

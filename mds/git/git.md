O que é o _Git_?
================

O _“Git”_ é um sistema de versionamento de aquivos, projetos, etc.
Existe uma [_playlist no YouTube_](https://youtu.be/WVLhm1AMeYE?list=PLInBAd9OZCzzHBJjLFZzRl6DgUmOeG3H0) de um curso no canal **RB Tech** que é extremamente recomendada para compreensão do _Git_.

## Instalação

Comando para instalar o _Git_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

```bash
$ sudo pacman -S git
```

Comando para instalar o _Git_ nos sistemas operacionais _Debian, Ubuntu, Linux Mint_:

```bash
$ sudo apt install git
```

## Configuração

Comandos para configurar as opções globais do _git_:

```bash
$ git config --global user.name "Seu Nome"
$ git config --global user.email "seunome@exemplo.com"
$ git config --list
```

## Bitbucket e GitHub

O _Bitbucket e o GitHub_ são serviços para hospedagem de repositórios _git_ e também uma espécie de rede social de programadores na qual podemos disponibilizar projetos, receber contribuições e também contribuir com outros projetos.
Basta criar uma conta no serviço que desejar e depois adicionar as chaves _ssh_ do computador no perfil.
Links para os serviços:

+ [bitbucket.org](http://bitbucket.org)
+ [github.com](http://github.com)

### xclip

Para facilitar a cópia da chave _ssh_ pode-se usar o _xclip_, que copia textos para área de transferência.

Comando para instalar o _xclip_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

```bash
$ sudo pacman -S xclip
```

Comando para instalar o _xclip_ nos sistemas operacionais _Debian, Ubuntu, Linux Mint_:

```bash
$ sudo apt install xclip
```

### Chaves SSH

Para autorizar o acesso as contas, ou no _Bitbucket_ ou no _GitHub_, a partir do
computador local sem a necessidade de informar usuário e senha para cada novo
acesso, basta criar um par de chaves _ssh_ e adicionar a chave publica na conta.

Com o _xclip_ instalado e com a conta configurada pode-se gerar as chaves _ssh_ com o seguinte comando:

```bash
$ ssh-keygen -t rsa -b 4096 -C "seunome@exemplo.com"
```

Comando para verificar a criação da chave _ssh_:

```bash
$ ls -la .ssh/
```

Comando para copiar a chave _ssh_ para a área de transferência usando o _xclip_:

```bash
$ xclip -sel clip < ~/.ssh/id_rsa.pub
```

Com a chave copiada, basta localizar a função de autorização de chaves no _Bitbucket_ ou no _GitHub_,

+ **Bitbucket:** `https://bitbucket.org/account/user/seu_usuario/ssh-keys/`
+ **GitHub:** `https://github.com/settings/keys`

Após adicionar a chave _ssh_ no _Bitbucket_ ou no _GitHub_, pode-se testar a conexão com o comando:

+ **Bitbucket**

```bash
$ ssh -vT git@bitbucket.org
```

+ **GitHub**

```bash
$ ssh -vT git@github.com
```
## Repositórios

Comando para iniciar um repositório local - em seu computador:

```bash
$ git init
```

Comando para relacionar um repositório local a um repositório criado em um dos serviços de hospedagem de repositório _Git_:

```bash
$ git remote add origin https://github.com/xxxxx/xxxxx.git
```

>"xxxxx" representa parte da url que aponta para o repositório especícfico que se deseja relacionar ao repositório local.

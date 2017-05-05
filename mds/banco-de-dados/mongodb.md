MongoDB
=======

_Versão 1 - atualizada em 05/05/2017_

-----

O _MongoDB_ é um banco de dados _NoSQL_.

## Instalação

Comando para instalar o _MongoDB_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

```bash
$ sudo pacman -S mongodb
```

Comandos para instalar o _MongoDB_ nos sistemas operacionais _Debian, Ubuntu, Linux Mint_:

```bash
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

echo "deb http://repo.mongodb.org/apt/debian jessie/mongodb-org/3.4 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

sudo apt-get update

sudo apt-get install -y mongodb-org
```

Comando para iniciar o _MongoDB_:

```bash
$ sudo systemctl start mongodb
```

Comando para acessar a interface de linha de comando do _MongoDB_:

```bash
$ mongo
```

Para sair da interface de linha de comando digite `CTRL + C`.

Comando para parar o _MongoDB_:

```bash
$ sudo systemctl start mongodb
```

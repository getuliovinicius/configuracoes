MongoDB
=======

O “MongoDB” é um banco de dados NoSQL.

## Instalação

Comando para instalar o _MongoDB_ nos sistemas operacionais _Arch Linux e Manjaro Linux_:

``` sh
$ sudo pacman -S mongodb
```

Comandos para instalar o _MongoDB_ nos sistemas operacionais _Debian, Ubuntu, Linux Mint_:

``` sh
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

echo "deb http://repo.mongodb.org/apt/debian jessie/mongodb-org/3.4 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

sudo apt-get update

sudo apt-get install -y mongodb-org
```

Comando para iniciar o _MongoDB_:

``` sh
$ sudo systemctl start mongod
```

Comando para acessar a interface de linha de comando do _MongoDB_:

``` sh
$ mongo
```

Para sair da interface de linha de comando digite `CTRL + C`.

Comando para parar o _MongoDB_:

``` sh
$ sudo systemctl start mongod
```


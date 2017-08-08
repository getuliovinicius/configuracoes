Servidor de Banco de Dados MariaDB
==================================

_Versão 1 - atualizada em 30/07/2017_

-----

O MariaDB é um sistema gerenciador de bases de dados "SGDB". É compatível com o MySQL e pode ser utilizado ao invés deste, como uma alternativa.

## Instalação do MariaDB

Sequência dos comandos para instalação o MariaDB no sistema operacional _Debian9 - Stretch_.

Criação do arquivos com a lista de espelhos:

```bash
$ sudo apt-get install software-properties-common
$ sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xF1656F24C74CD1D8
$ sudo vim /etc/apt/sources.list.d/MariaDB.list
```

As linhas abaixo devem ser inseridas no arquivo.

```
# MariaDB 10.1 repository list - created 2017-07-30 14:39 UTC
# http://downloads.mariadb.org/mariadb/repositories/
deb [arch=i386,amd64] http://mirror.ufscar.br/mariadb/repo/10.1/debian stretch main
deb-src http://mirror.ufscar.br/mariadb/repo/10.1/debian stretch main
```

Na sequencia a instalação do MariaDB

```
$ sudo apt update
$ sudo apt install mariadb-server
```

!!! note "Info"
	Durante a instalação do MariaDB é pedida a senha para o usuário **root**. Defina uma senha fácil, mas, se quer administrar esses trem já vai se acostumando a criar padrões de senha os quais você seja capaz de se lembrar.
	O exemplo abaixo é um método que remete a duas palavras: (MySQL local), portanto podemos, juntar as palavras, colocar um números antes e...
	**Senha para o usuário root:** mysqll0c4l

!!! note "Instalção em outras distribuições"
    O MariaDB pode ser instalado em diversos sistemas operacionais. Para ter maiores informações sobre como instalar em distribuições e versões diferentes de sistemas operacionais consulte o link:
    [https://downloads.mariadb.org/](https://downloads.mariadb.org/)
    A instalação demonstrada acima foi da versão 10.1. Quando for instalar deve-se atentar a versão mais recente disponível.

Instalação do MariaDB
=====================

libaio1 libdbd-mysql-perl libdbi-perl libhtml-template-perl libmysqlclient18 libterm-readkey-perl
  mysql-client-5.5 mysql-common mysql-server-5.5 mysql-server-core-5.5





**melhorar muito >>>**

O MariaDB é um sistema gerenciador de bases de dados “SGDB”.E comum a utilização do MariaDB em conjunto com o Apache e PHP para desenvolvimento web.
O MariaDB será utilizado por vários aplicativos escritos em PHP prontos que iremos instalar no servidor.

**<<< melhorar muito**

Para instalalar o _MariaDB_ usei o comando:

``` sh
$ sudo pacman -S mariadb
```

Durante a instalação do MyQSL é pedida a senha para o usuário root. Defina uma fácil mas se quer administrar esses trem já acostume-se a criar padrões de senha que vc seja capaz de lembrar. O exemplo abaixo é um método que remete a duas

palavras: (MariaDB local)

Senha para o usuário ROOT: “MariaDBl0c4l”

Para checar a versão do MariaDB:

``` sh
$ MariaDB --version
```

Para checar se o MariaDB está ativo rode o comando:

``` sh
$ sudo systemctl status MariaDB.service
```

Tornar Seguro o Servidor MariaDB

``` sh
$ sudo MariaDB_secure_installation
```

Agora tem que exercitar o inglês e interpretar corretamente o que está sendo pedido. Boa sorte.

Para testar o login:

``` sh
$ MariaDBadmin -p -u root version
```

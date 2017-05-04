Instalação do MySQL
=====================

O MySQL é um sistema gerenciador de bases de dados "SGDB". É comum a utilização do MySQL em conjunto com o Apache e PHP para desenvolvimento web.

## Instalação do _MySQL_

Comando para instalar o _MySQL_ nos sistemas operacionais _Debian, Ubuntu e Linux Mint_:

Para instalalar o _MySQL_ usei o comando:

```bash
$ sudo apt install mysql-server
```

!!! note "Info"
	**Pacotes instalados;** _libaio1 libdbd-mysql-perl libdbi-perl libhtml-template-perl libmysqlclient18 libterm-readkey-perl mysql-client-5.5 mysql-common mysql-server mysql-server-5.5 mysql-server-core-5.5_

	Durante a instalação do MyQSL é pedida a senha para o usuário root. Defina uma fácil mas se quer administrar esses trem já acostume-se a criar padrões de senha que vc seja capaz de lembrar. O exemplo abaixo é um método que remete a duas palavras: (MySQL local).
	Senha para o usuário ROOT: “mysqll0c4l”

Para checar a versão do MySQL:

```bash
$ mysql --version
```

Para checar se o MySQL está ativo rode o comando:

```bash
$ sudo systemctl status mysql.service
```

Tornar Seguro o Servidor MySQL

```bash
$ sudo mysql_secure_installation
```

Agora tem que exercitar o inglês e interpretar corretamente o que está sendo pedido. Boa sorte.

Para testar o login:

```bash
$ mysqladmin -p -u root version
```

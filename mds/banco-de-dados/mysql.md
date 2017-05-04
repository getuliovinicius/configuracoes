Instalação do MySQL
=====================

O MySQL é um sistema gerenciador de bases de dados "SGDB". É comum a utilização do MySQL em conjunto com o Apache e PHP para desenvolvimento web.

## Instalação do _MySQL_

Comando para instalar o _MySQL_ nos sistemas operacionais _Debian, Ubuntu e Linux Mint_:

```bash
$ sudo apt install mysql-server
```

!!! note "Info"
	**Pacotes instalados:** _libaio1 libdbd-mysql-perl libdbi-perl libhtml-template-perl libmysqlclient18 libterm-readkey-perl mysql-client-5.5 mysql-common mysql-server mysql-server-5.5 mysql-server-core-5.5_

	Durante a instalação do _MyQSL_ é pedida a senha para o usuário root. Defina uma senha fácil, mas, se quer administrar esses trem já vai se acostumando a criar padrões de senha os quais você seja capaz de se lembrar.
    O exemplo abaixo é um método que remete a duas palavras: (MySQL local), portanto podemos, juntar as palavras, colocar um números antes e...
	**Senha para o usuário root: “mysqll0c4l”**

Para checar a versão do MySQL execute o comando:

```bash
$ mysql --version
```

Para checar se o MySQL está ativo execute o comando:

```bash
$ sudo systemctl status mysql.service
```

!!! note "Info"
    Pode ser útil, caso esteja trabalhando em um servidor de produção, elevar o nível de segurança do servidor MySQL, para isso você pode recorrer ao utilitário `mysql_secure_installation`

    ```bash
    $ sudo mysql_secure_installation
    ```

    Responda as questões conforme sua necessidade.

Para testar o login use o comando:

```bash
$ mysqladmin -p -u root version
```
Phpmyadmin
==========

_Versão 1 - atualizada em 08/05/2017_

-----

O _Phpmyadmin_ é uma aplicação para gerenciar o banco de dados _MySQL_. É uma ferramenta fundamental pela facilidade que agrega as etapas de desenvolvimento.

## Requisitos

O _Phpmyadmin_ é uma aplicação web escrita em PHP e portanto necessita da linguagem PHP instalada e de um servidor, neste exemplo vamos usar o _Apache_, além disso obviamente necessita do banco de dados _MySQL_ instalado.

+ [Instalação do Apache](../servidores-web/apache.md)
+ [Instalação do MySQL](../banco-de-dados/mysql.md)
+ [Instalação do PHP](php.md)

## Instalação do Phpmyadmin

Primeiro é preciso baixar o pacote de instalação direto do site do desenvolvedor com o comando `wget`, vamos fazer isso direto no diretório de instalação:

```bash
$ cd /srv/www/php/
$ sudo wget https://files.phpmyadmin.net/phpMyAdmin/4.7.3/phpMyAdmin-4.7.3-all-languages.zip
```
!!! note "Info"
    Existe uma versão do _Phpmyadmin_ nos repositórios do _Ubuntu, Debian e Linux Mint_, mas vamos instalar na unha.
Descompactar o pacote de instalação e renomear o diretório:

```bash
$ sudo unzip phpMyAdmin-4.7.0-all-languages.zip
$ sudo mv phpMyAdmin-4.7.0-all-languages phpmyadmin.local
```

Na sequência é preciso editar o arquivo `config.inc.php`.

```bash
$ cd /srv/www/php/phpmyadmin.local
$ sudo cp config.sample.inc.php config.inc.php
$ sudo vim config.inc.php
```

Adicione uma senha na linha 16 do arquivo:

```bash
$cfg['blowfish_secret'] = 'phpmy4dm1n10293845876857phpmy4dm1n10293845876857phpmy4dm1n10293845876857'; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */
```

No exemplo foi utilizada a senha: `phpmy4dm1n10293845876857phpmy4dm1n10293845876857phpmy4dm1n10293845876857`.
Alterar o proprietário dos diretórios e arquivos em “/srv/www” para o usuário e grupo “www-data”

```bash
$ sudo chown www-data\: /srv/www -R
```

## Adicionar o _vHost_ ao Apache

Por último é preciso configurar o vhost.

```bash
$ cd /etc/apache2/sites-available
```

Criar um arquivo chamado `phpmyadmin.local.conf`:

```bash
$ sudo vim /etc/apache2/sites-available/phpmyadmin.local.conf
```

Copie o conteúdo abaixo para o arquivo:

```apacheconf
<VirtualHost *:80>
    DocumentRoot /srv/www/php/phpmyadmin.local
    <Directory /srv/www/php/phpmyadmin.local/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Order allow,deny
        allow from all
    </Directory>
    ServerName phpmyadmin.local
    ServerAdmin root@phpmyadmin.local
    ErrorLog /var/log/apache2/phpmyadmin.local_error.log
    CustomLog /var/log/apache2/phpmyadmin.local_access.log common
</VirtualHost>
```

O novo _vHost_ deve ser habilitado usando o comando `a2ensite` e o Apache reiniciado para persistir as mudança.

```bash
$ sudo a2ensite phpmyadmin.local.conf
$ sudo systemctl restart apache2.service
```

Finalmente adicionar a entrada para o _vlHost_ no arquivo `/etc/hosts`.

```bash
$ sudo vim /etc/hosts
```

Adicionar a linha abaixo ao final do arquivo:

```rc
127.0.0.1	phpmyadmin.local
```

## Usando o Phpmyadmin

Acesse o endereço [http://phpmyadmin.local](http://phpmyadmin.local) para verificar a instalação.
Através do _Phpmyadmin_ vamos criar um usuário chamado _gerente_app_, este usuário poderá ser utilizado para instalação de todas as aplicações e desenvolvimento de projetos locais que necessitem de base de dados.
Localize na interface o link `Contas de usuário` e crie a nova conta

+ **Nome de usuário:** gerente_app
+ **Host name:** localhost
+ **Senha:** G3r3nt3_4pp

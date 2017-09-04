Sistema de Tickets "HESK"
=========================

_Versão 1 - atualizada em 08/05/2017_

-----

O _Hesk_ é um sistema web escrito em PHP para geração de Tickets de suporte.

## Requisitos

O _Hesk_ é uma aplicação web escrita em PHP e portanto necessita da linguagem PHP instalada e de um servidor web, neste exemplo vamos usar o _Apache_ como servidor web, além disso necessita de uma baser de dados _MySQL_ instalada.

+ [Instalação do Apache](../servidores-web/apache.md)
+ [Instalação do MySQL](../banco-de-dados/mysql.md)
+ [Instalação do PHP](php-apache.md)

## Instalação do Hesk

Primeiro é preciso baixar o pacote de instalação direto do site do desenvolvedor [https://www.hesk.com/download.php](https://www.hesk.com/download.php)
Comandos para instalação do _Hesk_ - presumindo que o download foi feito no diretório padrão:

```bash
$ sudo mkdir -p /srv/www/php/tickets.local
$ cd /srv/www/php/tickets.local
$ sudo mv ~/Downloads/hesk273.zip .
```
Descompactar o pacote de instalação e renomear o diretório:

```bash
$ sudo unzip hesk273.zip
```

Alterar o proprietário dos diretórios e arquivos em `/srv/www` para o usuário e grupo _www-data_:

```bash
$ sudo chown www-data\: /srv/www -R
```

Crie um banco de dados MySQL para O _Hesk_. Você pode usar o _[Phpmyadmin](phpmyadmin.md)_ para criar o banco de dados;
Por último é preciso configurar o vhost.

```bash
$ cd /etc/apache2/sites-available
```

Criar um arquivo chamado `tickets.local.conf`:

```bash
$ sudo vim /etc/apache2/sites-available/tickets.local.conf
```

Copie o conteúdo abaixo para o arquivo:

```apacheconf
<VirtualHost *:80>
    DocumentRoot /srv/www/php/tickets.local
    <Directory /srv/www/php/tickets.local/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Order allow,deny
        allow from all
    </Directory>
    ServerName tickets.local
    ServerAdmin root@tickets.local
    ErrorLog /var/log/apache2/tickets.local_error.log
    CustomLog /var/log/apache2/tickets.local_access.log common
</VirtualHost>
```

O novo _vHost_ deve ser habilitado usando o comando `a2ensite` e o Apache reiniciado para persistir as mudança.

```bash
$ sudo a2ensite tickets.local.conf
$ sudo systemctl restart apache2.service
```

Finalmente adicionar a entrada para o _vlHost_ no arquivo `/etc/hosts`.

```bash
$ sudo vim /etc/hosts
```

Adicionar a linha abaixo ao final do arquivo:

```rc
127.0.0.1	tickets.local
```

Agora acesse o endereço: [http://tickets.local/install](http://tickets.local/install) e siga as instruções para configuração.

Após a instalação apague o diretório de instalação:

```bash
$ sudo rm -rf /srv/www/php/tickets.local/install
```

## Traduzir a interface do Hesk

Para traduzir a interface faça o download do pacote de tradução disponível em [https://www.hesk.com/language/](https://www.hesk.com/language/).
Comandos para traduzir a interface do _Hesk_ - presumindo que o download foi feito no diretório padrão:

```bash
$ cd /srv/www/php/tickets.local/language
$ sudo mv ~/Downloads/pt-BR.zip .
$ sudo unzip pt-BR.zip
```

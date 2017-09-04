Ambiente PHP - com Nginx
========================

_Versão 1 - atualizada em 30/07/2017_

-----

_PHP_ é uma linguagem de _scripts_ de propósito geral que é especialmente adequado para desenvolvimento web. _PHP_ é um acrônimo para "Pré Processor HiperText", mas e daí ;)

## Instalação do PHP

**Procedimento para instalar o _PHP_ no sistema operacional _Debian9 - Stretch:**

Partindo para a instalação, o comando abaixo vai instalar o básico do PHP:

```bash
$ sudo apt install php7.0-fpm php7.0-mysql php7.0-curl php7.0-mcrypt php7.0-mbstring php7.0-gd
```

!!! note "Info"
	**Pacotes instalados:** libmcrypt4 php-common php7.0-cli php7.0-common php7.0-curl php7.0-fpm php7.0-json php7.0-mbstring php7.0-mcrypt php7.0-mysql php7.0-opcache php7.0-readline

Para checar a instalação do PHP execute o comando:

```bash
$ php --version
```

Após a instalação, um parâmetro de configuração do PHP, `cgi.fix_pathinfo`, foi ajustado.

```bash
$ sudo vim /etc/php/7.0/fpm/php.ini
```

```
cgi.fix_pathinfo=0
```

```bash
$ sudo vim /etc/php/7.0/fpm/php.ini
```

## VirtualHost para o ambiente PHP

!!! note "Info"
	Totalmente opcional.

Para testar o funcionamento vamos criar um _VirtualHost_, ou simplesmente _vHost_, que sirva para checar o ambiente PHP e todas as suas características. Começaremos com a estrutura de diretórios para projetos de páginas dinâmicas em PHP.

```bash
$ sudo mkdir -p /srv/www/php/ambiente-php.local
$ cd /srv/www/php/ambiente-php.local
```

Crie um arquivo `info.php` dentro do diretório recém criado:

```bash
$ sudo vim index.php
```

Copie o conteúdo abaixo para o arquivo:

```php
<?php
phpinfo();
?>
```

Alterar o proprietário dos diretórios e arquivos em `/srv/www` para o usuário e grupo `nginx`:

```bash
$ sudo chown www-data\: /srv/www -R
```

Na sequência é preciso configurar o _vhost_.

```bash
$ cd /etc/nginx/sites-available
```

Criar um arquivo chamado `ambiente-php.local.conf`:

```bash
$ sudo vim /etc/nginx/sites-available/ambiente-php.local.conf
```

Copie o conteúdo abaixo para o arquivo:

```
server {

    # Port that the web server will listen on.
    listen 80;
    listen [::]:80;

    # Host that will serve this project.
    server_name ambiente-php.local;

    # Useful logs for debug.
    access_log /var/log/nginx/ambiente-php.local.access.log;
    error_log /var/log/nginx/ambiente-php.local.error.log;
    rewrite_log on;

    # The location of our projects public directory.
    root /srv/www/php/ambiente-php.local;

    # Point index to the app front controller.
    index index.php index.html;

    # URLs to attempt, including pretty ones.
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # Remove trailing slash to please routing system.
    if (!-d $request_filename) {
        rewrite ^/(.+)/$ /$1 permanent;
    }

    #
    #error_page 404 /404.html;

    # redirect server error pages to the static page /50x.html
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /srv/www/php/ambiente-php.local;
    }

    # PHP FPM configuration.
    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;

        # With php-fpm (or other unix sockets):
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
    #	# With php-cgi (or other tcp sockets):
    #	fastcgi_pass 127.0.0.1:9000;
    }

    #
    # We don't need .ht files with nginx.
    location ~ /\.ht {
        deny all;
    }

    # Set header expirations on per-project basis
    location ~* \.(?:ico|css|js|jpe?g|JPG|png|svg|woff)$ {
        expires 365d;
    }
}
```

O novo _vHost_ deve ser habilitado através da criação de um link simbólico em `/etc/nginx/sites-enabled`.

```bash
$ sudo ln -s /etc/nginx/sites-available/ambiente-php.local.conf /etc/nginx/sites-enabled/
```

E adicionar a entrada para o _vHost_ no arquivo `/etc/hosts`:

```bash
$ sudo vim /etc/hosts
```

Adicione a linha abaixo após as entradas criadas anteriormente.

```rc
127.0.0.1	ambiente-php.local
```

Acesse os endereços [http://ambiente-php.local](http://ambiente-php.local) para verificar toda a configuração do PHP.

## Gerenciador de dependências Composer

O _Composer_ pode ser instalado globalmente ou apenas no escopo do projeto que está em desenvolvimento. Siga as instruções abaixo para instalar globalmente, as mesmas podem podem ser vistas no site do gerenciador [https://getcomposer.org/](https://getcomposer.org/).

```bash
$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
$ php -r "if (hash_file('SHA384', 'composer-setup.php') === '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be6190fa0355160742ab2e1c88d40d5be660b410') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
$ php composer-setup.php
$ php -r "unlink('composer-setup.php');"
$ sudo mv composer.phar /usr/local/bin/composer
```

!!! attention "Atenção"
	A chave '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be
	6190fa0355160742ab2e1c88d40d5be660b410' pode não ser a mesma quando em uma instalação futura, é preciso conferir em [https://getcomposer.org/download/](https://getcomposer.org/download/).

Para verificar a versão do composer:

```bash
$ composer -V
```

## Configurar diretórios de desenvolvimento

No Linux o único diretório com permissão de escrita para o usuário padrão, o usuário da instalação do sistema operacional, é o diretório _home_ de sua conta: `/home/nome_usuario`.
Por essa razão não dá pra editar o código PHP no local que instalamos as aplicações, ou seja, o diretório `/srv/www/php`. Sendo assim vamos mudar as configurações de forma sutil para termos acesso de leitura e escrita aos arquivos de código fonte das aplicações.

!!! note "info"
	Em outros tutoriais irei demonstrar a instalação de aplicações como: _Drupal, Phpmyadmin, MediaWiki e WordPress_, tais aplicações não necessitam de alterações no código, portanto serão sempre instaladas no diretório `/srv/www/php`. A configuração mostrada até agora reflete de forma fidedigna as configurações ideais para instalação de aplicações PHP em servidores de produção. Daqui em diante veremos como transformar a instalação em algo próprio para desenvolvimento.

O primeiro passo é criar um diretório no diretório `home` do usuário padrão, criado durante a instalação do sistema operacional Linux, por exemplo `/home/nome_usuario/DEV_WEB/PHP/VHOSTS`.

```bash
$ mkdir -p ~/DEV_WEB/PHP/VHOSTS
```

!!! note "info"
	O `~` (til) é um curinga que substitui o diretório `home` no momento de digitar comandos no prompt.

Na pasta `VHOSTS` iremos criar propriamente nossas aplicações, portanto será ali que iremos colocar os diretórios `.local`, por exemplo:

```bash
$ cd ~/DEV_WEB/PHP/VHOSTS
$ mkdir projeto1.local
```

Agora vamos criar um link simbólico para o diretório _projeto1.local_ dentro do diretório reconhecido pelo Nginx `/srv/www/php/`:

```bash
$ cd /srv/www/php/
$ sudo ln -s ~/DEV_WEB/PHP/VHOSTS/projeto1.local .
$ sudo chown -R www-data\: projeto1.local
```

Acessamos o diretório de desenvolvimento criado na `home` do usuário padrão:

```bash
$ cd ~/DEV_WEB/PHP/VHOSTS/
```
Então passamos para a mudança sutil que nos permitirá o acesso com leitura e escrita aos códigos fonte de aplicações desenvolvidas em PHP.
Alteramos o usuário dono, mas não o grupo, do diretório `~/DEV_WEB/PHP/VHOSTS/projeto1.local` para o usuário padrão:

```bash
$ sudo chown -R seu_uruario:www-data projeto1.local/
```

De agora em diante é possível editar projetos, códigos de aplicações desenvolvidas em PHP, com editores de texto como o _Sublime Text_.
Sempre que for rodar uma aplicação desenvolvida por você mesmo repita os passos a seguir, substituindo _projetoX.local_ pelo nome que você deseja:

```bash
$ cd /srv/www/php/
$ sudo ln -s ~/DEV_WEB/PHP/VHOSTS/projetoX.local
$ sudo chown -R www-data\: projetoX.local
$ cd ~/DEV_WEB/PHP/VHOSTS
$ mkdir projetoX.local
$ sudo chown -R seu_uruario:www-data projetoX.local/
```

A configuração dos _vHosts_ e as entradas no arquivo `/etc/hosts` podem ser feitas conforme o exemplo em [VirtualHost para o ambiente PHP](php-nginx.md#virtualhost_para_o_ambiente_php).

!!! note "info"
	Acostume-se a lidar com permissões de arquivos, principalmente com os comandos `chown` e `chmod`, pois isso é fundamental no desenvolvimento e quem não sabe usar corretamente esses comandos faz cagada em servidores.
	Não é brincadeira. Saber usar o prompt de comando e conhecer o funcionamento de permissões e propriedades de arquivos e diretórios faz toda diferença.

Servidor Web Apache2
====================

_Versão 1 - atualizada em 01/05/2017_

-----

O Apache é o “servidor HTTP/servidor WEB” mais utilizado no mundo, atualmente está na versão 2.4.

## Instalação do Apache

Comando para instalar o _Apache_ nos sistemas operacionais _Debian, Ubuntu e Linux Mint_:

``` sh
$ sudo apt install apache2

# PACOTES INSTALADOS:
# apache2 apache2-bin apache2-data apache2-utils
# libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap liblua5.1-0
```

Para testar o funcionamento do Apache digite o seguinte endereço no navegador web:

[http://localhost](http://localhost)

Se retornar uma página com uma mensagem “It Works!” está tudo OK.
O diretório de hospedagem padrão do Apache no Ubuntu Linux é o `/var/www/html`. Neste diretório que ficam armazenados os arquivos que compõem os sites/aplicações web. O Diretório é chamado de `DocumentRoot`. Por enquanto existe apenas um `DocumentRoot` no sistema, que é exatamente o `localhost`, o endereço acessado para testar o funcionamento do Apache anteriormente.

## Módulos do Apache

Algumas aplicações em PHP irão solicitar módulos extra do Apache.
Para propiciar o uso de urls amigáveis, o `mod_rewrite` do Apache deve ser habilitado com o comando `a2enmod`.

``` sh
$ sudo a2enmod rewrite
```

Para ver os módulos do Apache que estão habilitados use o comando `apachectl`.

``` sh
$ sudo apachectl -M
```

## Demonstração do Apache

Entrar no diretório do `DocumentRoot` padrão do Apache:

``` sh
$ cd /var/www/html
```

Substituir o arquivo `index.html` mas primeiro fazer backup renomeando o arquivo `index.html` para `index.html.original` com o comando `mv`.

``` sh
$ sudo mv index.html index.html.original
```

Criar um novo arquivo `index.html` com o editor _vim_ para demonstração do funcionamento do Apache.

``` sh
$ sudo vim index.html
```

Copiar o conteúdo abaixo para o arquivo.

``` html
<!DOCTYPE HTML>
<html lang="pt-br">

	<head>
		<meta charset="UTF-8">
		<!-- <link rel="stylesheet" type="text/css" href="style.css"> -->
		<title>Localhost - Página padrão</title>
	</head>

	<body>
		<h1>Página padrão</h1>
		<p>Página padrão do servidor web. O Apache está funcionando.</p>
		<p>##### VIVA!!! #####</p>
		<p><a href='https://github.com/getuliovinicius' title='Good!!!'>GitHub do Getulio Vinicius</a></p>
	</body>

</html>
```

Acesse o endereço [http://localhost](http://localhost) para ver o resultado da mudança.

Criar o arquivo `style.css`.

``` sh
$ sudo vim style.css
```

Copiar o conteúdo abaixo para o arquivo:

``` css
@charset "utf-8";
/* CSS Document */
body {
	background-color: #e8eae8;
	color: #5d665b;
	margin: 50px;
}
p {
	text-align: center;
}
a:link {
	color: #696;
	text-decoration: none;
	background-color: transparent
}
a:visited {
	color: #699;
	text-decoration: none;
	background-color: transparent
}
a:hover {
	color: #c93;
	text-decoration: underline;
	background-color: transparent
}
a:active {
	color: #900;
	text-decoration: underline;
	background-color: transparent
}
```


Agora para aplicar o estilo a página remova o comentário da linguagem HTML na linha 6 do arquivo `index.html`, que são as tags `<!-- -->` no início e no final da linha.

``` sh
$ sudo vim index.html
```

Retire os comentários e acesse o endereço [http://localhost](http://localhost) para ver o resultado.

## Configuração do _VirtualHost_ Padrão

O apache será utilizado aproveitando a funcionalidade de _VirtualHosts_.
O primeiro passo é a criação da estrutura de diretórios a partir do diretório `/srv`, isto porque segundo a hierarquia de arquivos FHS (que eu não vou explicar, não faz diferença agora), o diretório próprio para instalação de serviços no Linux é o `/srv`.
Enfim, sobre essa questão, de quando eu fiz o curso de _Sysadmin_ até os dias atuais, as distribuições passaram a configurar o `DocumentRoot` no diretório `/var/www`. Mudar ou não essa configuração não faz muita diferença mas pode ser um bom exercício para entender o funcionamento do Linux.

``` sh
$ cd
$ sudo mkdir -p /srv/www
```

Para páginas estáticas em HTML vamos deixar definido que os `vhosts` ficarão no diretório `/srv/www/html`.

Mover o conteúdo do diretório `/var/www/` para `/srv/www/`.

``` sh
$ sudo mv /var/www/html /srv/www/
```

Vamos então estruturar o servidor:

Dentro do diretório `/srv/www/html` vamos criar outro diretório chamado `localhost` onde iremos armazenar os arquivos que editamos até agora.

``` sh
$ cd /srv/www/html
$ sudo mkdir localhost
```

Agora indo para o diretório `localhost` vamos  copiar o arquivo padrão da instalação do Apache, que renomeamos para `index.html.original`, juntamente com os arquivos `index.html` e `style.css`:

``` sh
$ cd /srv/www/html/localhost
$ sudo mv /srv/www/html/index.* .
$ sudo mv /srv/www/html/style.css .
```

Na sequência precisamos definir o usuário `www-data`, que é o usuário sob o qual o serviço Apache é executado, como dono dos arquivos sob o diretório `/srv/www` inclusive o próprio diretório `/srv/www`. Para realizar essa tarefa usamos o comando `chown`:

``` sh
$ sudo chown www-data\: /srv/www -R
```

Neste momento o servidor Apache em funcionamento, vai exibir uma mensagem de erro, iniciada com "Not Found", não encontrado, devido a alteração do local onde por padrão os arquivos do `DocumentRooot` são instalados.
Tendo sido realizada a alteração do local dos arquivos referentes ao diretório `DocumentRoot` passamos para a configuração do Apache, acessando o diretório de configuração.

``` sh
$ cd /etc/apache2
```

Neste diretório podemos modificar a configuração padrão do servidor Apache deixando de acordo com os nossos propósitos.
A primeira configuração que iremos aplicar é a indicação da diretiva `ServerName`, o nome padrão do servidor, que vamos manter como localhost.

``` sh
$ cd conf-available
```

Usando o vim podemos criar o arquivo de configuração do `ServerName` padrão que irá possuir apenas uma linha.

``` sh
$ sudo vim servername.conf
```

``` conf
ServerName localhost
```

Depois de criar o arquivo é preciso adicionar a nova configuração ao Apache com o comando `a2enconf`.

``` sh
$ sudo a2enconf servername
```

Após informando o `ServerName` padrão, é preciso informar que mudamos o diretório `DocumentRoot` para que ele volte a fornecer as páginas referentes ao endereço [http://localhost](http://localhost). Vamos voltar um diretório com o comando `cd`.

``` sh
$ cd ..
```

O aquivo de configuração `apache2.conf`, deve ser copiado para `apache2.conf.original`.

``` sh
$ sudo cp apache2.conf apache2.conf.original
```

Na sequência o arquivo `apache2.conf` deve ser alterado removendo o comentário `#` das linhas 170, 171, 172, 173 e 174. Isso fará com que o Apache enxergue os arquivos no diretório `/srv/www`.

``` sh
$ sudo vim apache2.conf
```
``` conf
<Directory /srv/>
	Options Indexes FollowSymLinks
	AllowOverride None
	Require all granted
</Directory>
```

E por fim vem a configuração do `VirtualHost` padrão do Apache, o `localhost`.
O arquivo de configuração do `VirtualHost` padrão é o `000-default.conf` localizado no diretório `/etc/apache2/sites-available/`. Este diretório também irá armazenar os arquivos de configuração de todos os demais `VirtualHosts` que por ventura vierem a ser criados.

``` sh
$ cd /etc/apache2/sites-available
```

Fazer o backup do arquivo `000-default.conf`:

``` sh
$ sudo cp 000-default.conf 000-default.conf.original
```

Os seguinte parâmetros devem ser alterados no arquivo `000-default.conf`:

``` sh
$ sudo vim /etc/apache2/sites-available/000-default.conf
```

Linhas 12 e 13

``` conf
#DocumentRoot /var/www/html
DocumentRoot /srv/www/html/localhost
```

Reiniciar o Apache:

``` sh
$ sudo systemctl restart apache2.service
```

Acesse o endereço [http://localhost](http://localhost) para ver o resultado.

## Diretórios dos VirtualHosts para Projetos

Vamos criar dois `VirtualHosts` para exemplificar o funcionamento do Apache, ambos podem ser utilizados para estudos das linguagens HTML, Javascript e CSS, lembrando que por enquanto estamos trabalhando na estrutura de diretórios criada para páginas estáticas.
Criar o diretório chamado `/srv/www/html/app1.local`:

``` sh
$ sudo mkdir /srv/www/html/app1.local
```

Entrar no diretório `/srv/www/html/app1.local`:

``` sh
$ cd /srv/www/html/app1.local/
```

Criar os arquivos “index.html” e “style.css”:

``` sh
$ sudo vim index.html
```

Copie o conteúdo abaixo no arquivo:

``` html
<!DOCTYPE HTML>
<html lang="pt-br">

	<head>
		<meta charset="UTF-8">
		<link rel="stylesheet" type="text/css" href="style.css">
		<title>APP-1</title>
	</head>

	<body>
		<h1>Meu aplicativo 1</h1>
		<p>Esse é o meu aplicativo nro 1.</p>
		<p><a href='https://github.com/getuliovinicius' title='Good!!!'>GitHub do Getulio Vinicius</a></p>
	</body>

</html>
```

``` sh
$ sudo vim style.css
```

Copie o conteúdo abaixo no arquivo:

``` css
@charset "UTF-8";
/* CSS Document */
body {
	background-color: #e8eae8;
	color: #5d665b;
	margin: 50px;
}
p {
	text-align: center;
}
a:link {
	color: #696;
	text-decoration: none;
	background-color: transparent
}
a:visited {
	color: #699;
	text-decoration: none;
	background-color: transparent
}
a:hover {
	color: #c93;
	text-decoration: underline;
	background-color: transparent
}
a:active {
	color: #900;
	text-decoration: underline;
	background-color: transparent
}
```

Depois de criados os dois arquivos, vamos subir um nível na hierarquia de diretórios criada:

``` sh
$ cd ..

#ou

$ cd /srv/www/html
```

Vamos copiar o diretório `app1.local` para `app2.local`:

``` sh
$ sudo cp -R app1.local app2.local
```

O parâmetro `-R` serve para copiar de forma recursiva, todos os arquivos abaixo do diretório de origem.
Entre no diretório `app2.local`:

``` sh
$ cd app2.local
```

Edite os arquivos `index.html` e `style.css`:

``` sh
$ sudo vim index.html
```

Copie o conteúdo abaixo no arquivo:

``` html
<!DOCTYPE HTML>
<html lang="pt-br">

	<head>
		<meta charset="UTF-8">
		<link rel="stylesheet" type="text/css" href="style.css">
		<title>APP-2</title>
	</head>

	<body>
		<h1>Meu aplicativo 2</h1>
		<p>Esse é o meu aplicativo nro 2.</p>
		<p><a href='https://github.com/getuliovinicius' title='Good!!!'>GitHub do Getulio Vinicius</a></p>
	</body>

</html>
```

``` sh
$ sudo vim style.css
```

Copie o conteúdo abaixo no arquivo:

``` css
@charset "utf-8";
/* CSS Document */
body {
	background-color: #e8eae8;
	color: #000;
	margin: 100px;
}
p {
	text-align: center;
}
a:link {
	color: #696;
	text-decoration: none;
	background-color: transparent
}
a:visited {
	color: #699;
	text-decoration: none;
	background-color: transparent
}
a:hover {
	color: #c93;
	text-decoration: underline;
	background-color: transparent
}
a:active {
	color: #900;
	text-decoration: underline;
	background-color: transparent
}

```
Alterar o proprietário dos diretórios e arquivos em “/srv/www” para o usuário e grupo “www-data”

``` sh
$ sudo chown www-data\: /srv/www -R
```

## Configuração dos _VirtualHosts_

``` sh
$ cd /etc/apache2/sites-available
```

Criar um arquivo chamado `app1.local.conf` com o conteúdo:

``` sh
$ sudo vim /etc/apache2/sites-available/app1.local.conf
```

``` conf
<VirtualHost *:80>
	DocumentRoot /srv/www/html/app1.local
	<Directory /srv/www/html/app1.local/>
		Options Indexes FollowSymLinks
		AllowOverride All
		Order allow,deny
 		allow from all
	</Directory>
	ServerName app1.local
	ServerAdmin root@app1.local
	ErrorLog /var/log/apache2/app1.local_error.log
	CustomLog /var/log/apache2/app1.local_access.log common
</VirtualHost>
```

Copiar o arquivo `app1.local.conf` para um novo arquivo `app2.local.conf`:

``` sh
$ sudo cp app1.local.conf app2.local.conf
```

Editar o arquivo `app2.local.conf`:

``` sh
$ sudo vim /etc/apache2/sites-available/app2.local.conf
```

``` conf
<VirtualHost *:80>
	DocumentRoot /srv/www/html/app2.local
	<Directory /srv/www/html/app2.local/>
		Options Indexes FollowSymLinks
		AllowOverride All
		Order allow,deny
		allow from all
	</Directory>
	ServerName app2.local
	ServerAdmin root@app2.local
	ErrorLog /var/log/apache2/app2.local_error.log
	CustomLog /var/log/apache2/app2.local_access.log common
</VirtualHost>
```

Alterar o proprietário dos diretórios e arquivos em `/srv/www` para o usuário e grupo `www-data`.

``` sh
$ sudo chown www-data\: /srv/www -R
```

Os dois novos `VirtualHosts` devem ser habilitados usando o comando `a2ensite`, e o Apache reiniciado para persistirem as mudanças.

``` sh
$ sudo a2ensite app1.local.conf
$ sudo a2ensite app2.local.conf
$ sudo systemctl restart apache2.service
```

Para concluir tudo é preciso criar as entradas para os `VirtualHosts` no arquivo `/etc/hosts`.

``` sh
$ sudo vim /etc/hosts
```

``` conf
127.0.0.1	localhost
127.0.1.1	UBUNTU-NOTE-01
# a linha abaixo são as entradas para as aplicações criadas.
127.0.0.1	app1.local
127.0.0.1	app2.local
```

Acesse os endereços [http://app1.local](http://app1.local) e [http://app2.local](http://app2.local), para verificar o resultado.

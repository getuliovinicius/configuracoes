Servidor Web Apache2
====================

_Versão 1 - atualizada em 04/05/2017_

-----

O Apache é o “servidor HTTP/servidor WEB” mais utilizado no mundo, atualmente está na versão 2.4.

## Instalação do Apache

Comando para instalar o _Apache_ nos sistemas operacionais _Debian, Ubuntu e Linux Mint_:

```bash
$ sudo apt install apache2
```
!!! note "Info"
	**Pacotes instalados:** apache2 apache2-data apache2-utils

Para testar o funcionamento do Apache digite o seguinte endereço [http://localhost](http://localhost) no navegador web. Se retornar uma página com uma mensagem “It Works!” está tudo OK.

## Módulos do Apache

Algumas aplicações poderão solicitar módulos extra do Apache, por exemplo: para propiciar o uso de urls amigáveis, o módulo `mod_rewrite` do Apache deve ser habilitado e para habilitar um módulo usamos o comando `a2enmod`.

```bash
$ sudo a2enmod rewrite
```
!!! note "Info"
	O comando acima habilita o módulo _mod_rewrite_ no Apache.

Para ver os módulos do Apache que estão habilitados podemos usar o comando `apachectl`.

```bash
$ sudo apachectl -M
```

## Demonstração do Apache

O diretório de publicação padrão do Apache no Debian é o `/var/www/html`, neste diretório ficam armazenados os arquivos que compõem os sites hospedados no servidor.
O diretório `/var/www/html` é chamado de _DocumentRoot_ do _VirtualHost_ padrão do Apache, o _[localhost](http://localhost)_, endereço citado anteriormente para testar o funcionamento do Apache. Cada _VirtualHost_ possui seu próprio diretório _DocumentRoot_ conforme veremos mais adiante.
Para entender a estrutura do Apache vamos entrar no diretório _DocumentRoot_ do _VirtualHost_ padrão e listar o conteúdo:

```bash
$ cd /var/www/html
$ ls
```

Existe um arquivo chamado `index.html` que contém a codificação da página exibida quando requisitamos o endereço `localhost`. Vamos começar a alterar a estrutura original do Apache substituindo o conteúdo do arquivo `index.html`, mas primeiro devemos fazer um backup deste arquivo renomeando de `index.html` para `index.html.original` com o comando `mv`.

```bash
$ sudo mv index.html index.html.original
```

Em seguida criar um novo arquivo `index.html` com o editor _vim_.

```bash
$ sudo vim index.html
```

Copiar o conteúdo abaixo para o arquivo.

```html
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

Para conferir acessamos o endereço [http://localhost](http://localhost) novamente.
Vamos tratar o visual da página criando um arquivo `style.css`.

```bash
$ sudo vim style.css
```

Copiar o conteúdo abaixo para o arquivo:

```css
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

Agora para aplicar o estilo a página removemos o comentário da linguagem HTML na linha 6 do arquivo `index.html`, que são as tags `<!-- -->` no início e no final da linha.

```bash
$ sudo vim index.html
```

Retire os comentários e acesse novamente o endereço [http://localhost](http://localhost) para ver o resultado.

## Configuração do _VirtualHost_ Padrão

O apache utiliza a funcionalidade de _VirtualHosts_, a qual permite a manter vários sites ativos simultâneamente, ou seja, vários sites compartilham uma instalação.
O primeiro passo é a criação da estrutura de diretórios a partir do diretório `/srv`, isto porque segundo a hierarquia de arquivos FHS (que eu não vou explicar, não faz diferença agora), o diretório próprio para instalação de serviços no Linux é o `/srv`.
Enfim, sobre essa questão, de quando eu fiz o curso de _Sysadmin_ até os dias atuais, as distribuições passaram a configurar o _DocumentRoot_ no diretório `/var/www`. Mudar ou não essa configuração não faz muita diferença mas pode ser um bom exercício para entender o funcionamento do Linux.

```bash
$ cd
$ sudo mkdir -p /srv/www
```

Para páginas estáticas em HTML vamos deixar definido que os _VirtualHosts_, `vHosts`, ficarão no diretório `/srv/www/html`.
Mover o conteúdo do diretório `/var/www/` para `/srv/www/`.

```bash
$ sudo mv /var/www/html /srv/www/
```

Podemos então estruturar o servidor. Dentro do diretório `/srv/www/html` vamos criar outro diretório chamado `localhost` onde iremos armazenar os arquivos que editamos até agora.

```bash
$ cd /srv/www/html
$ sudo mkdir localhost
```

Agora indo para o diretório `localhost` vamos  copiar o arquivo padrão da instalação do Apache, que renomeamos para `index.html.original`, juntamente com os arquivos `index.html` e `style.css`:

```bash
$ cd /srv/www/html/localhost
$ sudo mv /srv/www/html/index.* .
$ sudo mv /srv/www/html/style.css .
```

Na sequência precisamos definir o usuário _www-data_, que foi criado durante a instalação do Apache para execução do serviço, como usuário dono dos arquivos sob o diretório `/srv/www` inclusive o próprio diretório `/srv/www`. Para realizar essa tarefa usamos o comando `chown`:

```bash
$ sudo chown www-data\: /srv/www -R
```

Neste momento o servidor Apache, que está em funcionamento, vai exibir uma mensagem de erro, iniciada com "Not Found", não encontrado, devido a alteração do local onde por padrão os arquivos do _DocumentRoot_ são instalados.
Tendo sido realizada a alteração do local dos arquivos referentes ao diretório _DocumentRoot_ passamos para a configuração do Apache, acessando o diretório de configuração.

```bash
$ cd /etc/apache2
```

Neste diretório podemos modificar a configuração padrão do servidor Apache deixando de acordo com os nossos propósitos.
A primeira configuração que iremos aplicar é a indicação da diretiva `ServerName`, o nome padrão do servidor, que vamos manter como `localhost`.

```bash
$ cd conf-available
```

Usando o _vim_ podemos criar o arquivo de configuração do `ServerName` padrão que irá possuir apenas uma linha.

```bash
$ sudo vim servername.conf
```

```apacheconf
ServerName localhost
```

Depois de criar o arquivo é preciso adicionar a nova configuração ao Apache com o comando `a2enconf`.

```bash
$ sudo a2enconf servername
```

Após informando o `ServerName` padrão, é preciso informar que mudamos o diretório _DocumentRoot_ para que ele volte a fornecer as páginas referentes ao endereço [http://localhost](http://localhost).
Vamos voltar um diretório com o comando `cd`.

```bash
$ cd ..
```

O aquivo de configuração `apache2.conf`, deve ser copiado para `apache2.conf.original`.

```bash
$ sudo cp apache2.conf apache2.conf.original
```

Na sequência o arquivo `apache2.conf` deve ser alterado removendo o comentário `#` das linhas 170, 171, 172, 173 e 174. Isso fará com que o Apache enxergue os arquivos no diretório `/srv/www`.

```bash
$ sudo vim apache2.conf
```

Linhas descomentadas:

```apacheconf
<Directory /srv/>
	Options Indexes FollowSymLinks
	AllowOverride None
	Require all granted
</Directory>
```

E por fim vem a configuração do `vHost` padrão do Apache, o `localhost`.
O arquivo de configuração do `vHost` padrão é o `000-default.conf` localizado no diretório `/etc/apache2/sites-available/`. Este diretório também irá armazenar os arquivos de configuração de todos os demais `vHosts` que por ventura vierem a ser criados.

```bash
$ cd /etc/apache2/sites-available
```

Fazer o backup do arquivo `000-default.conf`:

```bash
$ sudo cp 000-default.conf 000-default.conf.original
```

Os seguinte parâmetros devem ser alterados no arquivo `000-default.conf`:

```bash
$ sudo vim /etc/apache2/sites-available/000-default.conf
```

Linhas 12 e 13

```apacheconf
#DocumentRoot /var/www/html
DocumentRoot /srv/www/html/localhost
```

Reiniciar o Apache:

```bash
$ sudo systemctl restart apache2.service
```

Por fim acessamos o endereço [http://localhost](http://localhost) para ver o resultado.

## Diretórios dos _vHost_ para Projetos

Vamos criar dois `vHost` para exemplificar o funcionamento do Apache, ambos podem ser utilizados para estudos das linguagens HTML, Javascript e CSS, lembrando que por enquanto estamos trabalhando na estrutura de diretórios criada para páginas estáticas.
Criar o diretório chamado `/srv/www/html/app1.local`:

```bash
$ sudo mkdir /srv/www/html/app1.local
```

Entrar no diretório `/srv/www/html/app1.local`:

```bash
$ cd /srv/www/html/app1.local/
```

Criar os arquivos `index.html` e `style.css`:

```bash
$ sudo vim index.html
```

Copie o conteúdo abaixo no arquivo:

```html
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

```bash
$ sudo vim style.css
```

Copie o conteúdo abaixo no arquivo:

```css
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

```bash
$ cd ..
```

ou

```bash
$ cd /srv/www/html
```

Vamos copiar o diretório `app1.local` para `app2.local`:

```bash
$ sudo cp -R app1.local app2.local
```

O parâmetro `-R` serve para copiar de forma recursiva, todos os arquivos abaixo do diretório de origem.
Entre no diretório `app2.local`:

```bash
$ cd app2.local
```

Edite os arquivos `index.html` e `style.css`:

```bash
$ sudo vim index.html
```

Copie o conteúdo abaixo no arquivo:

```html
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

```bash
$ sudo vim style.css
```

Copie o conteúdo abaixo no arquivo:

```css
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

Alterar o proprietário dos diretórios e arquivos em `/srv/www` para o usuário e grupo `www-data`:

```bash
$ sudo chown www-data\: /srv/www -R
```

## Configuração dos _vHost_

```bash
$ cd /etc/apache2/sites-available
```

Criar um arquivo chamado `app1.local.conf` com o conteúdo:

```bash
$ sudo vim /etc/apache2/sites-available/app1.local.conf
```

Copiar o conteúdo abaixo para o arquivo:

```apacheconf
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

```bash
$ sudo cp app1.local.conf app2.local.conf
```

Editar o arquivo `app2.local.conf`:

```bash
$ sudo vim /etc/apache2/sites-available/app2.local.conf
```

Copiar o conteúdo abaixo para o arquivo:

```apacheconf
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

Os dois novos `vHosts` devem ser habilitados usando o comando `a2ensite`, e o Apache reiniciado para persistirem as mudanças.

```bash
$ sudo a2ensite app1.local.conf
$ sudo a2ensite app2.local.conf
$ sudo systemctl restart apache2.service
```

### Arquivo “/etc/hosts”

Para o ambiente de desenvolvimento pretendido é preciso atribuir uma entrada no arquivo `/etc/hosts`, para cada domínio local que se deseja acessar.
Essa ação fará com que os navegadores web instalados no computador possam acessar páginas locais, servidas pelo Apache, a partir de domínios locais.
No exemplo abaixo existe uma entrada para o domínio local, _app1.local_, que pode ser acessado no navegador com o endereço [http://app1.local](http://app1.local).
Mostar o conteúdo do arquivo `/etc/hosts`:

```
$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	DEBIAN-01 # este nome é definido na instalação
```

Para concluir tudo é preciso criar as entradas para os `vHost` no arquivo `/etc/hosts`.

```bash
$ sudo vim /etc/hosts
```

```
127.0.0.1	localhost
127.0.1.1	DEBIAN-01 # este nome é definido na instalação

# As linhas abaixo são as entradas para as aplicações criadas.
127.0.0.1	app1.local
127.0.0.1	app2.local
```

Acesse os endereços [http://app1.local](http://app1.local) e [http://app2.local](http://app2.local), para verificar o resultado.

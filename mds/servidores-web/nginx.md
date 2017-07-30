Servidor Web Nginx
==================

_Versão 1 - atualizada em 30/07/2017_

-----

O Nginx é um “servidor HTTP/servidor WEB” moderno, que esta sendo amplamente adotado.

## Instalação do _Nginx_

Comando para instalar o _Nginx_ nos sistemas operacionais _Debian9 - Stretch_:

```bash
$ wget http://nginx.org/keys/nginx_signing.key
$ sudo apt-key add nginx_signing.key
$ sudo vim /etc/apt/sources.list.d/Nginx.list
```

As linhas abaixo devem ser inseridas no arquivo.

```
deb http://nginx.org/packages/debian/ stretch nginx
deb-src http://nginx.org/packages/debian/ stretch nginx
```

```bash
$ sudo apt update
$ sudo apt install nginx
$ sudo /lib/systemd/systemd-sysv-install enable nginx
$ sudo systemctl start nginx.service
```

!!! note "Instalção em outras distribuições"
    O _Nginx_ pode ser instalado em diversos sistemas operacionais. Para ter maiores informações sobre como instalar em distribuições e versões diferentes de sistemas operacionais consulte o link:
    [http://nginx.org/en/linux_packages.html#distributions](http://nginx.org/en/linux_packages.html#distributions)

## Configuração

Para utilizar o _Nginx_ em um ambiente Desktop para desenvolvimento aproveitamos a funcionalidade de “VirtualHosts”.
O primeiro passo foi a criação da estrutura de diretórios a partir do diretório `/srv`, isto porque segundo a hierarquia de arquivos FHS o diretório próprio para instalação de serviços no Linux é o `/srv`.

```bash
$ cd
$ sudo mkdir -p /srv/www
```

Para páginas estáticas em HTML vamos deixar definido que os “vhosts” ficarão no diretório “/srv/www/html”.

O conteúdo do diretório `/usr/share/nginx/html` foi copiado para `/srv/www/`.

```bash
$ sudo mv /usr/share/nginx/html /srv/www/
```

Dentro do diretório `html` foi criado outro diretório chamado `localhost` onde armazenamos os arquivos do _vhost_ padrão.

```bash
$ sudo mkdir /srv/www/html/localhost
```

Indo para o diretório `localhost` copiamos os arquivos `.html` padrão da instalação do _Nginx_, `index.html` e `50x.html`:

```bash
$ sudo mv /srv/www/html/*.html /srv/www/html/localhost
```

Na sequência definimos o usuário `nginx`, que é o usuário sob o qual o serviço _Nginx_ é executado, como dono dos arquivos sob o diretório `/srv/www` inclusive o próprio diretório `/srv/www`.

Para realizar essa tarefa usamos o comando `chown`:

```bash
$ sudo chown nginx\: /srv/www -R
```

Após toda essa mudança, ao acessar o servidor _Nginx_ em funcionamento, será exibida uma mensagem de erro, iniciada com “Not Found”, não encontrado, devido a alteração do local onde por padrão os arquivos do _DocumentRoot_, os aquivos da raiz do site padrão, são instalados.
Tendo sido realizada a alteração do local dos arquivos referentes ao diretório _DocumentRoot_ passamos para a configuração do _Nginx_, acessando o diretório de configuração.

```bash
$ cd /etc/nginx
```

Neste diretório modificamos a configuração padrão do servidor _Nginx_ deixando de acordo com os nossos propósitos.
O arquivo de configuração `nginx.conf`, foi copiado para `nginx.conf.original`”.

```bash
$ sudo cp nginx.conf nginx.conf.original
$ sudo vim nginx.conf
```

A linha incluindo as configurações presentes no diretório `sites-enabled` foi adicionada antes do fechamento da chave `}` na sessão `http`.

```
include /etc/nginx/sites-enabled/*.conf;
```

Por fim vem a configuração do _VirtualHost_ padrão do _Nginx_, o `localhost`. O arquivo de configuração do _VirtualHost_ padrão é o `default.conf` localizado no diretório `/etc/nginx/conf.d`.

```bash
$ cd /etc/nginx/conf.d
$ sudo vim /etc/nginx/conf.d/default.conf
```

O parâmetro `root` do arquivo `default.conf` foi alterado tanto na sessão do diretório raiz do site, `location /`, quanto na sessão dos erros 50X.

```
...
location / {
    #root   /usr/share/nginx/html;
    root   /srv/www/html/localhost;
    index  index.html index.htm;
}
...
error_page   500 502 503 504  /50x.html;
location = /50x.html {
    #root   /usr/share/nginx/html;
    root   /srv/www/html/localhost;
}
...
```

Na sequência devem ser criados os diretórios `/etc/nginx/sites-available` e `/etc/nginx/sites-enabled`.

```bash
$ sudo mkdir /etc/nginx/sites-available
$ sudo mkdir /etc/nginx/sites-enabled
```

Estes diretórios são para a criação de configurações de todos os demais _VirtualHosts_ que porventura vierem a ser criados.
Em `/etc/nginx/sites-available` devem ficar os links simbólicos que apontam para arquivos em `/etc/nginx/sites-available`.

## Diretórios dos _vHost_ para Projetos

Vamos criar dois `vHost` para exemplificar o funcionamento do _Nginx_, ambos podem ser utilizados para estudos das linguagens HTML, Javascript e CSS, lembrando que por enquanto estamos trabalhando na estrutura de diretórios criada para páginas estáticas.
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
$ sudo chown nginx\: /srv/www -R
```

## Configuração dos _vHost_

```bash
$ cd /etc/nginx/sites-available
```

Criar um arquivo chamado `app1.local.conf`:

```bash
$ sudo vim /etc/nginx/sites-available/app1.local.conf
```

As configuraçõe abaixo foram copiadas para o arquivo:

```
server {
    listen       80;
    server_name  app1.local;

    access_log  /var/log/nginx/app1.local.access.log  main;

    location / {
        root   /srv/www/html/app1.local;
        index  index.html;
    }

    # redirect server error pages to the static page /50x.html
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /srv/www/html/app1.local;
    }

    # deny access to .htaccess files, if Apache's document root concurs with nginx's one
    location ~ /\.ht {
        deny  all;
    }
}
```

Criar um arquivo chamado `app2.local.conf` com o conteúdo:

```bash
$ sudo vim /etc/nginx/sites-available/app2.local.conf
```

As configuraçõe abaixo foram copiadas para o arquivo:

```
server {
    listen       80;
    server_name  app2.local;

    access_log  /var/log/nginx/app2.local.access.log  main;

    location / {
        root   /srv/www/html/app2.local;
        index  index.html;
    }

    # redirect server error pages to the static page /50x.html
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /srv/www/html/app2.local;
    }

    # deny access to .htaccess files, if Apache's document root concurs with nginx's one
    location ~ /\.ht {
        deny  all;
    }
}
```

Os dois novos _vHosts_ devem ser habilitados através da criação de links simbólicos em `/etc/nginx/sites-enabled`.

```bash
$ sudo ln -s /etc/nginx/sites-available/app1.local.conf /etc/nginx/sites-enabled/
$ sudo ln -s /etc/nginx/sites-available/app2.local.conf /etc/nginx/sites-enabled/
```

## Arquivo hosts

Para o ambiente de desenvolvimento pretendido é preciso atribuir uma entrada no arquivo `/etc/hosts`, para cada domínio local que se deseja acessar.
Essa ação fará com que os navegadores web instalados no computador possam acessar páginas locais, servidas pelo _Nginx_, a partir de domínios locais.
No exemplo abaixo existe uma entrada para o domínio local, _app1.local_, que pode ser acessado no navegador com o endereço [http://app1.local](http://app1.local).
Mostar o conteúdo do arquivo `/etc/hosts`:

```
$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	DEBIAN-01 # este nome é definido na instalação
```

Para concluir tudo é preciso criar as entradas para os _vHost_ no arquivo `/etc/hosts`.

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

## Conclusão

Testar as configurações do _Nginx_:

```bash
$ sudo nginx -t
```

Se tudo estiver correto, reiniciar o serviço _Nginx_:

```bash
$ sudo systemctl restart nginx
```

Acesse os endereços [http://app1.local](http://app1.local) e [http://app2.local](http://app2.local), para verificar o resultado.    

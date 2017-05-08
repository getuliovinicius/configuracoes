Configuração do Framework Laravel
(http://laravel.com/)
O Laravel é um framework escrito em PHP que facilita o desenvolvimento de aplicações web. Existem diversos cursos no YouTube para iniciar os estudos, aqui eu vou mostrar como instalar apenas.
Banco de Dados
É preciso criar uma base de dados no MySQL para utilização em projetos. Utilize o Phpmyadmin para criar uma base de dados para a instalação do Laravel.
usuário: gerente_app
senha: G3r3nt3_4pp
base de dados: laravel_01
Download e Instalação do Laravel
Vamos iniciar criando o link simbólico do vhost:
$ cd /srv/www/php/
$ sudo ln -s ~/DEV_WEB/VHOSTS/projeto-laravel-01.local
$ sudo chown -R www-data\: projeto-laravel-01.local
Acessar o diretório de hospedagem dos arquivos de aplicações PHP na estrutura de diretórios criada na home do usuário padrão:
$ cd ~/DEV_WEB/VHOSTS
No site do projeto Laravel, indicado antes, você encontra o link para documentação do framework. Seguiremos as instruções e faremos a instalação através do terminal do Linux utilizando o comando “composer”:
$ composer create-project --prefer-dist laravel/laravel projeto-laravel-01.local
O nome projeto-laravel-01.local é apenas um exemplo.
Alterar o dono e o grupo dos arquivos respectivamente para: seu_usuario e www-data:
$ sudo chown -R seu_usuario:www-data projeto-laravel-01.local/
Adicionar o VirtualHost ao Apache
É preciso criar um arquivo de configuração do VirtualHost, no diretório “/etc/apache2/sites-available” e a entrada no arquivo “/etc/hosts” da mesma forma como foram criadas para as aplicações de teste APP1 e APP2.
Editar o arquivo “projeto2.local.conf”:
$ sudo vim /etc/apache2/sites-available/projeto-laravel-01.local.conf
<VirtualHost *:80>
		DocumentRoot /srv/www/php/projeto-laravel-01.local/public
		<Directory /srv/www/vhosts/projeto-laravel-01.local/public/>
   		Options Indexes FollowSymLinks
   		AllowOverride All
   		Order allow,deny
   		allow from all
		</Directory>
		ServerName projeto-laravel-01.local
		ServerAdmin root@projeto-laravel-01.local
		ErrorLog /var/log/apache2/projeto-laravel-01.local_error.log
		CustomLog /var/log/apache2/projeto-laravel-01.local_access.log common
</VirtualHost>
O novo VirtualHost deve ser habilitado usando o comando “a2ensite”, e o Apache reiniciado para assimilar as mudanças.
$ sudo a2ensite projeto-laravel-01.local.conf
$ sudo systemctl restart apache2.service
Para concluir tudo é preciso criar as entradas no arquivo “/etc/hosts”.
$ sudo vim /etc/hosts
127.0.0.1	localhost
127.0.1.1	UBUNTU-NOTE-01
# as linhas abaixo são as entradas para as aplicações criadas.
...
127.0.0.1	projeto-laravel-01.local
Acesse o endereço “http://projeto-laravel-01.local” para ver o resultado do desenvolvimento da aplicação.
Para editar o código da aplicação abra o Sublime (instalação mostrada no final), vá em File > Open Folder, localize o diretório “~/DEV_WEB/VHOSTS/projeto-laravel-01.local”, e abra-o.

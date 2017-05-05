PHPMYADMIN
O Phpmyadmin é uma aplicação para gerenciar o banco de dados MySQL. É uma ferramenta fundamental para o desenvolvimento Web pela facilidade que agrega as etapas de desenvolvimento.
Instalação do Phpmyadmin:
Acessar o diretório de hospedagem dos projetos em PHP:
$ cd /srv/www/php/
Primeiro é preciso baixar o pacote de instalação direto do site do desenvolvedor com o comando “wget”:
$ sudo wget
 https://files.phpmyadmin.net/phpMyAdmin/4.6.6/phpMyAdmin-4.6.6-all-languages.zip
Existe uma versão do Phpmyadmin nos repositórios do Ubuntu, mas vamos instalar na unha.
Descompactar o pacote de instalação e renomear o diretório:
$ sudo unzip phpMyAdmin-4.6.3-all-languages.zip
$ sudo mv phpMyAdmin-4.6.3-all-languages phpmyadmin.local
Na sequência é preciso editar o arquivo ”config.inc.php”.
$ cd /srv/www/php/phpmyadmin.local
$ sudo cp config.sample.inc.php config.inc.php
$ sudo vim config.inc.php
Adicione uma senha na linha 16 do arquivo,:
$cfg['blowfish_secret'] = 'phpmy4dm1n10293845876857phpmy4dm1n10293845876857phpmy4dm1n10293845876857'; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */
No exemplo foi utilizado:
 “phpmy4dm1n10293845876857phpmy4dm1n10293845876857phpmy4dm1n10293845876857”
como senha.
Alterar o proprietário dos diretórios e arquivos em “/srv/www” para o usuário e grupo “www-data”
$ sudo chown www-data\: /srv/www -R
Adicionar o VirtualHost ao Apache
Por último é preciso configurar o vhost.
$ cd /etc/apache2/sites-available
Criar um arquivo chamado “phpmyadmin.local.conf”:
$ sudo vim /etc/apache2/sites-available/phpmyadmin.local.conf
Copie o conteúdo abaixo para o arquivo:
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
O novo VirtualHost deve ser habilitado usando o comando “a2ensite”, e o Apache reiniciado para persistir as mudança.
$ sudo a2ensite phpmyadmin.local.conf
$ sudo systemctl restart apache2.service
Finalmente adicionar a entrada para o VirtualHost no arquivo “/etc/hosts”.
$ sudo vim /etc/hosts
Adicione a linha abaixo após as entradas criadas anteriormente para APP1 e APP2.
127.0.0.1	phpmyadmin.local
Usando o Phpmyadmin
Acesse os endereços “http://phpmyadmin.local” para verificar toda a configuração do PHP.
Através do Phpmyadmin vamos criar um usuário chamado “gerente_app”, que será utilizado para instalação de todas as aplicações e desenvolvimento de projetos locais que necessitem de base de dados.
PERCEBE QUE DESSE JEITO VC PASSA A ENTENDER MELHOR O FUNCIONAMENTO DAS COISAS? RETORNANDO AO QUE PONTUEI NO INÍCIO, DOCKER É UMA BOA, MAS AQUI NESSE PONTO VOCÊ JÁ DEVE TER PERCEBIDO QUE SABER COMO ORGANIZAR O SISTEMA É BEM INTERESSANTE.
Localize na interface o link “Contas de usuário” e crie a nova conta
Nome de usuário: gerente_app
Host name: localhost
Senha: G3r3nt3_4pp

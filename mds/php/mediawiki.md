MEDIAWIKI
(https://www.mediawiki.org/wiki/MediaWiki)
O MediaWikki é uma aplicação em PHP colcaborativa para formação de bases de conhecimento.
É preciso criar ma base de dados no MySQL para instalação da aplicação. Utilize o Phpmyadmin para criar uma base de dados para a instalação do MediaWiki e conceda todas as permissões ao usuário “gerente_app”.
usuário: gerente_app
senha: G3r3nt3_4pp
base de dados: wiki_01
$ sudo apt install php-apcu php-apcu-bc
Instalação do MediaWiki
Acessar o diretório de hospedagem dos arquivos de aplicações PHP na estrutura de diretórios em “/srv”.
$ cd /srv/www/php
No site do projeto MediaWiki, indicado antes, você encontra o link para download do pacote de instalação da versão estável, compactado no formato tar.gz. Copie o link e faça o download através do terminal do Linux utilizando o comando “wget”:
$ sudo wget https://releases.wikimedia.org/mediawiki/1.27/mediawiki-1.27.0.tar.gz
Para descompactar o pacote use o utilitário “tar”:
$ sudo tar -xzvf mediawiki-1.27.0.tar.gz
Agora vamos renomear o diretório descompactado para qualquer coisa .local:
$ sudo mv mediawiki-1.27.0 wiki.local
O nome wiki.local é apenas um exemplo.
É preciso atualizar o arquivo “images/.htaccess” no diretório de instalação do MediaWiki, “wiki.local”:
$ cd /srv/www/php/wiki.local/images
$ sudo vim .htaccess
Copie o conteúdo abaixo no inicio do arquivo:
# Serve HTML as plaintext, don't execute SHTML
AddType text/plain .html .htm .shtml .php .phtml .php5
# Old way of registering php with AddHandler
RemoveHandler .php
# Recent way of registering php with SetHandler
<FilesMatch "\.ph(p[345]?s?|tml)$">
   SetHandler None
</FilesMatch>
# If you've other scripting languages, disable them too.
Alterar o proprietário dos diretórios e arquivos em “/srv/www” para o usuário e grupo “www-data”
$ sudo chown www-data\: /srv/www -R
Adicionar o VirtualHost ao Apache
É preciso criar um arquivo de configuração do VirtualHost, no diretório “/etc/apache2/sites-available” e a entrada no arquivo “/etc/hosts” da mesma forma como foram criadas para as aplicações de teste APP1 e APP2.
Após a criação do VirtualHost e da entrada no arquivo “/etc/hosts” é preciso adicionar as configurações do Apache com o comando “a2ensite”:
$ sudo a2ensite wiki.local.conf
Reiniciar o apache:
$ sudo systemctl restart apache2.service
Para iniciar a instalação do MediaWiki no servidor local acesse: “http://wiki.local”
Durante a instalação do MediaWiki será solicitada a configuração do nome do site do usuário administrador, senha e e-mail.
usuário: admin
e-mail: admin@wiki.local
senha: *6oc(oi92ZozFSyYod
Extensões apcu e apcu-bc
Caso não dê certo a tentativa de instalar o MEDIAWIKI e durante a instalação ele reclame da falta de extensões do php, existe uma grande chance de ser a “apcu”. Instale com o comando abaixo e depois volte para a instalação da aplicação.
Quando concluir a instalação será realizado o download do arquivo de configuração que deve ser enviado para a raiz do MediaWiki:
$ cd /srv/www/php/wiki.local
$ sudo mv ~/Downloads/LocalSettings.php .
Alterar o proprietário dos diretórios e arquivos em “/srv/www” para o usuário e grupo “www-data”
$ sudo chown www-data\: /srv/www -R

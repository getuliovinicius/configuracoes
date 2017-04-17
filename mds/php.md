Configuração do PHP
PHP é uma linguagem de scripts de propósito geral popular que é especialmente adequado para desenvolvimento web.
PHP é um acrônimo para Pré Processor Hiper Text. Mas e daí ;)
Instalação do PHP
Partindo para a instalação, o comando abaixo vai instalar o básico do PHP em seu desktop Ubuntu 16.04.
$ sudo apt install php7.0
PACOTES INSTALADOS: php-common php7.0 php7.0-cli php7.0-common php7.0-fpm php7.0-json php7.0-opcache php7.0-readline
Para checar a versão do PHP:
$ php --version
Instalação do módulo PHP para o Apache
Vamos instalar o módulo do apache “libapache2-mod-php”
$ sudo apt-get install libapache2-mod-php7.0
PACOTES INSTALADOS: libapache2-mod-php7.0
VirtualHost para o ambiente PHP
Para testar o funcionamento vamos criar um VirtualHost que sirva para checar o ambiente PHP e todas as suas características.
Começaremos com a estrutura de diretórios para projetos de páginas dinâmicas em PHP.
$ sudo mkdir -p /srv/www/php/ambiente-php.local
$ cd /srv/www/php/ambiente-php.local
Crie um arquivo “info.php” dentro do diretório recém criado:
$ sudo vim index.php
Copie o conteúdo abaixo para o arquivo:
<?php
phpinfo();
?>
Alterar o proprietário dos diretórios e arquivos em “/srv/www” para o usuário e grupo “www-data”
$ sudo chown www-data\: /srv/www -R
Na sequência é preciso configurar o vhost.
$ cd /etc/apache2/sites-available
Criar um arquivo chamado “ambiente-php.local.conf”:
$ sudo vim /etc/apache2/sites-available/ambiente-php.local.conf
Copie o conteúdo abaixo para o arquivo:
<VirtualHost *:80>
DocumentRoot /srv/www/php/ambiente-php.local
		<Directory /srv/www/php/ambiente-php.local/>
	Options Indexes FollowSymLinks
   		AllowOverride All
   		Order allow,deny
   		allow from all
	</Directory>
		ServerName ambiente-php.local
		ServerAdmin root@ambiente-php.local
		ErrorLog /var/log/apache2/ambiente-php.local_error.log
		CustomLog /var/log/apache2/ambiente-php.local_access.log common
</VirtualHost>
O novo VirtualHost deve ser habilitado usando o comando “a2ensite”, e o Apache reiniciado para persistir as mudança.
$ sudo a2ensite ambiente-php.local.conf
$ sudo systemctl restart apache2.service
E adicionar a entrada para o VirtualHost no arquivo “/etc/hosts”.
$ sudo vim /etc/hosts
Adicione a linha abaixo após as entradas criadas anteriormente para APP1 e APP2.
127.0.0.1	ambiente-php.local
Acesse os endereços “http://ambiente-php.local” para verificar toda a configuração do PHP.
Extensões do PHP que são Dependências de Muitas Aplicações
Observação: As extensões do PHP devem ser instaladas e configuradas apenas para satisfazer as necessidades de aplicações que se queira usar. Não devem ser instalados módulos que não serão utilizados por nenhuma aplicação. As extensões que serão instaladas na sequência, são exigências para instalar aplicações como Phpmyadmin, MediaWiki, Drupal e também para desenvolver utilizando os Frameworks: Laravel e CodeIgniter por exemplo.
gd, mbstring, mcrypt, mysql, intl, xml
$ sudo apt install php7.0-gd php7.0-mbstring php7.0-mcrypt php7.0-mysql php7.0-intl php7.0-xml
PACOTES INSTALADOS: libmcrypt4 php7.0-gd php7.0-intl php7.0-mbstring php7.0-mcrypt php7.0-mysql php7.0-xml
curl
Extensão necessária para quase tudo que envolve de webservices.
$ sudo apt install curl php7.0-curl
PACOTES INSTALADOS: php7.0-curl
Listar as configurações do PHP
Após instalar as extensões do PHP que lhe forem necessárias, é preciso reiniciar o Apache:
$ sudo systemctl restart apache2.service
Para listar os módulos do PHP instalados e habilitados, rode o comando:
$ php -m
Gerenciadores dependências e pacotes
Gerenciador de dependências “PEAR”: (opcional)
$ sudo apt-get install php-pear
PACOTES INSTALADOS: php-pear
Reiniciar o Apache:
$ sudo systemctl restart apache2.service
Composer
O composer pode ser instalado globalmente como explicado logo abaixo, mas eu prefiro instalar apenas no escopo do projeto que está em desenvolvimento.
As instruções para uso podem ser vistas aqui: https://getcomposer.org/
Siga as instruções abaixo para instalar globalmente:
$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
$ php -r "if (hash_file('SHA384', 'composer-setup.php') === 'e115a8dc7871f15d853148a7fbac7da27d6c0030b848d9b3dc09e2a0388afed865e6a3d6b3c0fad45c48e2b5fc1196ae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
$ php composer-setup.php
$ php -r "unlink('composer-setup.php');"
$ sudo mv composer.phar /usr/local/bin/composer
Configuração de Aplicações PHP prontas para uso
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
WORDPRESS
(https://br.wordpress.org/)
O WordPress é uma aplicação em PHP para gerenciamento de conteúdo CMS (em inglês content management system).
É preciso criar uma base de dados no MySQL para instalação da aplicação. Utilize o Phpmyadmin para criar uma base de dados para a instalação do WordPress e conceda todas as permissões ao usuário “gerente_app”.
usuário: gerente_app
senha: G3r3nt3_4pp
base de dados: wordpress_01
Instalação do WordPress
Acessar o diretório de hospedagem dos arquivos de aplicações PHP na estrutura de diretórios em “/srv”.
$ cd /srv/www/php
No site do projeto WordPress, indicado antes, você encontra o link para download do pacote de instalação da versão estável, compactado no formato tar.gz. Copie o link e faça o download através do terminal do Linux utilizando o comando “wget”:
$ sudo wget https://br.wordpress.org/wordpress-4.7.1-pt_BR.tar.gz
Para descompactar o pacote use o utilitário “tar”:
$ sudo tar -xzvf wordpress-4.7.1-pt_BR.tar.gz
Agora vamos renomear o diretório descompactado para qualquer coisa .local:
$ sudo mv wordpress wordpress-01.local
O nome “wordpress-01.local” é apenas um exemplo, poderia ser “blog.local”.
Alterar o proprietário dos diretórios e arquivos em “/srv/www” para o usuário e grupo “www-data”
$ sudo chown www-data\: /srv/www -R
Adicionar o VirtualHost ao Apache
É preciso criar um arquivo de configuração do VirtualHost, no diretório “/etc/apache2/sites-available” e a entrada no arquivo “/etc/hosts” da mesma forma como foram criadas para as aplicações de teste APP1 e APP2.
Após a criação do VirtualHost e da entrada no arquivo “/etc/hosts” é preciso adicionar as configurações do Apache com o comando “a2ensite”:
$ sudo a2ensite wordpress-01.local.conf
Reiniciar o apache:
$ sudo systemctl restart apache2.service
Para iniciar a instalação do WordPress no servidor local acesse: “http://wordpress-01.local”
Durante a instalação do WordPress será solicitada a configuração do nome do site do usuário administrador, senha e e-mail.
usuário: admin
e-mail: admin@blog.local
senha: *6oc(oi92ZozFSyYod
Repositório de extensões PHP
Para conseguir instalar todas as dependências, ou pelo menos a maioria delas, necessárias a algumas aplicações conceituadas escritas em PHP, sem a necessidade de compilar extensões na unha, você pode adicionar um repositório que possui uma vasta lista de pacotes dedicado a complementos da linguagem PHP.
Observação: Adicione o repositório abaixo apenas se você tem intenção de instalar alguns CMS como o Drupal ou o MediaWiki.
Use o comando abaixo para adicionar o repositório:
$ sudo add-apt-repository ppa:ondrej/php
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
DRUPAL
“Drupal é um software de gerenciamento de conteúdo. É usado para fazer muitos dos sites e aplicativos que você usa todos os dias. Drupal tem grandes recursos padrão, como criação fácil de conteúdo, desempenho confiável e excelente segurança. Mas o que o diferencia é a sua flexibilidade; modularidade é um dos seus princípios fundamentais. Suas ferramentas ajuda a construir o conteúdo versátil, estruturados que as experiências Web dinâmicas precisam.” - (https://www.drupal.org/)
Banco de dados
É preciso criar uma base de dados no MySQL para instalação da aplicação. Utilize o Phpmyadmin para criar uma base de dados para a instalação do Drupal.
usuário: gerente_app
senha: G3r3nt3_4pp
base de dados: drupal_01
Instalação do Drupal
Acessar o diretório de hospedagem dos arquivos de aplicações PHP na estrutura de diretórios em “/srv”.
$ cd /srv/www/php
No site do projeto Drupal, indicado antes, você encontra o link para download do pacote de instalação da versão estável compactado no formato “tar.gz”. Copie o link e faça o download através do terminal do Linux utilizando o comando “wget”:
$ sudo wget https://ftp.drupal.org/files/projects/drupal-8.0.2.tar.gz
Para descompactar o pacote use o utilitário “tar”:
$ sudo tar -xzvf drupal-8.0.1.tar.gz
Agora vamos renomear o diretório descompactado para qualquer coisa .local:
$ sudo mv drupal-8.0.1 aplicacao.local
O nome aplicacao.local é apenas um exemplo.
Permissões para os Diretórios e Arquivos de Configuração
Para inciar a instalação do Drupal é preciso criar os arquivos “../site/default/settings.php” e “../site/default/services.yml”. Ambos podem ser copiados de arquivos padrões que já existem no diretório.
$ cd /srv/www/php/aplicacao.local/sites/default/
$ sudo cp default.settings.php settings.php
$ sudo cp default.services.yml services.yml
Ajustar as permissões dos dois arquivos para que possam ser escritos durante a instalação do Drupal:
$ sudo chmod a+w se*
Criar o diretório de traduções:
$ cd /srv/www/php/aplicacao.local/
$ sudo mkdir -p sites/default/files/translations
Alterar o proprietário dos diretórios e arquivos em “/srv/www” para o usuário e grupo “www-data”
$ sudo chown www-data\: /srv/www -R
Adicionar o VirtualHost ao Apache
É preciso criar um arquivo de configuração do VirtualHost, no diretório “/etc/apache2/sites-available” e a entrada no arquivo “/etc/hosts” da mesma forma como foram criadas para as aplicações de teste APP1 e APP2.
Após a criação do VirtualHost e da entrada no arquivo “/etc/hosts” é preciso adicionar as configurações do Apache com o comando “a2ensite”:
$ sudo a2ensite aplicacao.local.conf
Reiniciar o apache:
$ sudo systemctl restart apache2.service
Para inciar a instalação do Drupal no servidor local acesse: “http://aplicacao.local”
Durante a instalação do Drupal será solicitada a configuração do nome do site do usuário administrador, senha e e-mail.
usuário: admin
e-mail: admin@aplicacao.local
senha: *6oc(oi92ZozFSyYod
Extensões
php-dev (opcional)
$ sudo apt-get install php-dev
PACOTES INSTALADOS: autoconf automake autotools-dev debhelper dh-php dh-strip-nondeterminism gettext intltool-debian libarchive-zip-perl libasprintf-dev libfile-stripnondeterminism-perl libgettextpo-dev libgettextpo0 libltdl-dev libmail-sendmail-perl libpcre3-dev libpcre32-3 libpcrecpp0v5 libssl-dev libssl-doc libssl1.0.2 libsys-hostname-long-perl libtool m4 php7.0-dev pkg-php-tools po-debconf shtool xml2 zlib1g-dev
Por questões de segurança o pacote “php-dev” deve ser removido após a instalação das bibliotecas compiladas em servidores que estejam em produção. No caso de ambientes de desenvolvimento como este, pode se manter o pacote, pois podemos precisar de algumas dependências para compilar bibliotecas.
uploadprogress (necessária para o Drupal)
$ sudo apt-get install uploadprogress
php-twig (necessária para o Drupal)
$ sudo apt-get install php-twig
Configurar Aplicações em Diretórios Acessíveis
No Linux o único diretório com permissão de escrita para o usuário padrão, o usuário da instalação do sistema operacional, é o diretório “home” de sua conta: “/home/nome_usuario”.
Por essa razão não dá pra editar o código PHP no local que decidimos instalar as aplicações, ou seja, o diretório “/srv/www/php”. Sendo assim vamos mudar as configurações de forma sutil para termos acesso de leitura e escrita aos arquivos de código fonte das aplicações.
As aplicações: Drupal, Phpmyadmin, MediaWiki e WordPress mostradas acima não necessitam de alterações no código, portanto não sofrerão alterações nas permissões. A configuração mostrada até agora reflete de forma fidedigna as configurações ideais para instalação de aplicações PHP em servidores de produção.
Configuração do Diretório de Desenvolvimento
O primeiro passo é criar um diretório no diretório home do usuário padrão, criado durante a instalação do sistema operacional Linux, por exemplo “/home/nome_usuario/DEV_WEB/VHOSTS”:
$ mkdir -p ~/DEV_WEB/VHOSTS
O “~” (til) é um curinga que substitui o diretório home no momento de digitar comandos no prompt.
Na pasta projetos iremos criar propriamente nossas aplicações, portanto será ali que iremos colocar os diretórios “.local”, por exemplo:
$ cd ~/DEV_WEB/VHOSTS
$ mkdir projeto1.local
Agora vamos criar um link simbólico para o diretório “projeto1.local” dentro do diretório reconhecido pelo Apache “/srv/www/php/”:
$ cd /srv/www/php/
$ sudo ln -s ~/DEV_WEB/VHOSTS/projeto1.local .
$ sudo chown -R www-data\: projeto1.local
Acessamos o diretório de desenvolvimento criado na home do usuário padrão:
$ cd ~/DEV_WEB/VHOSTS/
Então passamos para a mudança sutil que nos permitirá o acesso com leitura e escrita aos códigos fonte de aplicações desenvolvidas em PHP.
Alteramos o usuário dono, mas não o grupo, do diretório “~/DEV_WEB/VHOSTS/projeto1.local” para o usuário padrão:
$ sudo chown -R seu_uruario:www-data projeto1.local/
De agora em diante é possível editar projetos, códigos de aplicações desenvolvidas em PHP, com editores de texto como o Sublime Text que iremos instalar logo mais.
Sempre que for rodar uma aplicação desenvolvida por você mesmo repita os passos na sequência, substituindo “projetoX.local” pelo nome que você deseja:
$ cd /srv/www/php/
$ sudo ln -s ~/DEV_WEB/VHOSTS/projetoX.local
$ sudo chown -R www-data\: projetoX.local
$ cd ~/DEV_WEB/VHOSTS
$ mkdir projetoX.local
$ sudo chown -R seu_uruario:www-data projetoX.local/
A configuração dos VirtualHosts e as entradas no arquivo “/etc/hosts” podem ser feitas conforme os exemplos anteriores.
Veja um exemplo prático na configuração do primeiro projeto em Laravel, logo mais.
Acostume-se a lidar com permissões de arquivos, principalmente com os comandos chown e chmod, pois isso é fundamental no desenvolvimento e quem não sabe usar corretamente esses comandos faz cagada em servidores.
Não é brincadeira. Saber usar o prompt de comando e conhecer o funcionamento de permissões e propriedades de arquivos e diretórios faz toda diferença.
Se você não se empolgou com isso, fica então esses dois scripts, um para criar um novo projeto e outro para remover um projeto.
Basta copiar o código e salvar em um arquivo dentro de um diretório de sua preferência, eu aconselho o deixar em “~/DEV_WEB/” e logo após dar permissão de execução,
novo-projeto.sh
$ cd ~/DEV_WEB
$ vim novo-projeto.sh
#!/bin/bash
# RECEBE O USUARIO ATUAL
USER_DEV=$USER
# LE O NOME DO DOMINIO PARA O PROJETO
echo "Informe o dominio para o projeto (Ex.: projeto-01.local):"
read DOMINIO
if [ -e "/etc/apache2/sites-available/$DOMINIO.conf" ]; then
    echo "ERRO: O dominio já existe."
    exit 1
fi
# LE A LINGUAGEM DO PROJETO
echo "Informe a linguagem principal de desenvolvimento do projeto (Ex.: php):"
read LINGUAGEM
# CRIA O DIRETORIO DO PROJETO
#mkdir -pv $HOME/DEV_WEB/VHOSTS/$DOMINIO/
# MUDAR PARA SUPERUSUARIO
echo ">>>"
echo "Para criar o vHost no Apache é preciso rodar o script como super usuario."
echo "Certifique-se que seu usuario ($USER_DEV) tem permissao para usar o comando sudo."
echo ">>>"
sudo -u root bash << EOF
# ALTERA A PROPRIEDADE DO GRUPO DO DIRETORIO DO PROJETO PARA O GRUPO DO APACHE
chown -v $USER_DEV:www-data $HOME/DEV_WEB/VHOSTS/$DOMINIO
# CRIA UM LINK ENTRE DO DIRETORIO DO PROJETO PARA O DIRETORIO DO VHOST
ln -s $HOME/DEV_WEB/VHOSTS/$DOMINIO /srv/www/$LINGUAGEM/$DOMINIO
chown -Rv www-data\: /srv/www/$LINGUAGEM/$DOMINIO
# CRIA O VHOST DO APACHE
echo "<VirtualHost *:80>
    DocumentRoot /srv/www/$LINGUAGEM/$DOMINIO
    <Directory /srv/www/$LINGUAGEM/$DOMINIO/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Order allow,deny
        allow from all
    </Directory>
    ServerName $DOMINIO
    ServerAdmin root@$DOMINIO
    ErrorLog /var/log/apache2/$DOMINIO-error.log
    CustomLog /var/log/apache2/$DOMINIO-access.log common
</VirtualHost>" > /etc/apache2/sites-available/$DOMINIO.conf
a2ensite $DOMINIO.conf
# REINICIA O APACHE
systemctl restart apache2.service
# CRIA UMA ENTRADA PARA O DOMINIO NO ARQUIVO HOSTS
echo "127.0.0.1     $DOMINIO" >> /etc/hosts
EOF
echo "acesse: http://$DOMINIO"
exit
$ chmod +x novo-projeto.sh
Para chamar na linha de comando digite:
$ ./novo-projeto.sh
remover-projeto.sh
$ cd ~/DEV_WEB
$ vim remover-projeto.sh
#!/bin/bash
# RECEBE O USUARIO ATUAL
USER_DEV=$USER
# LE O NOME DO DOMINIO PARA O PROJETO
echo "Informe o projeto que quer remover (Ex.: projeto-01.local):"
read DOMINIO
if [ ! -e "/etc/apache2/sites-available/$DOMINIO.conf" ]; then
    echo "ERRO: O dominio não existe."
    exit 1
fi
# LE A LINGUAGEM DO PROJETO
echo "Informe a linguagem principal de desenvolvimento do projeto (Ex.: php):"
read LINGUAGEM
if [ ! -d "/srv/www/$LINGUAGEM/$DOMINIO" ]; then
    echo "ERRO: Link do projeto não existe."
    exit 1
fi
# MUDAR PARA SUPERUSUARIO
echo ">>>"
echo "Para remover o vHost no Apache é preciso rodar o script como super usuario."
echo "Certifique-se que seu usuario ($USER_DEV) tem permissao para usar o comando sudo."
echo ">>>"
sudo -u root bash << EOF
# REMOVE A ENTRADA PARA O DOMINIO NO ARQUIVO HOSTS
sed -i '/$DOMINIO/Id' /etc/hosts
# REMOVE O VHOST DO APACHE
a2dissite $DOMINIO.conf
rm /etc/apache2/sites-available/$DOMINIO.conf
# REMOVE O LINK SIMBOLICO ENTRE O DIRETORIO DO PROJETO E O VHOST
rm /srv/www/$LINGUAGEM/$DOMINIO
# REINICIA O APACHE
systemctl restart apache2.service
EOF
# REMOVE O DIRETORIO DO PROJETO
rm -rfv $HOME/DEV_WEB/VHOSTS/$DOMINIO/
$ chmod +x remover-projeto.sh
Para chamar na linha de comando digite:
$ ./remover-projeto.sh
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
Configurando os serviços para inicialização
Não é necessário manter os seRviços funcionando no seu Desktop caso não esteja desenvolvendo, sendo assim fica a dica:
Remova da inicialização do sistema os serviços “mysql, php7.0-fpm e apache2”:
$ sudo systemctl disable mysql.service
$ sudo systemctl disable php7.0-fpm.service
$ sudo systemctl disable apache2.service
Crie o arquivo “iniciar-ambiente-web”:
$ sudo vim iniciar-ambiente-web
# mysql
systemctl start mysql.service
# apache
systemctl start apache2.service
# php
systemctl start php7.0-fpm.service
Crie o arquivo “parar-ambiente-web”:
$ sudo vim iniciar-ambiente-web
# mysql
systemctl stop mysql.service
# apache
systemctl stop apache2.service
# php
systemctl stop php7.0-fpm.service
Mova os arquivos “parar-ambiente-web e iniciar-ambiente-web” para um diretório de sistema:
$ sudo mv *-ambiente-web /usr/local/bin/
Quando quiser desenvolver execute:
$ sudo iniciar-ambiente-web
Quando quiser parar de desenvolver momentaneamente execute:
$ sudo parar-ambiente-web
Pau no gato.
E isso é tudo sobre PHP no momento...
Aplicativos para Auxiliar no Desenvolvimento
Editor de Textos Sublime
O Editor de textos “Sublime” é uma boa ferramenta para lidar com projetos de aplicações.
Pense na possibilidade de comprar uma licença deste software.
Instalação do Editor “Sublime”
$ sudo add-apt-repository ppa:webupd8team/sublime-text-3
$ sudo apt-get update
$ sudo apt-get install sublime-text
Sistema de Controle de Versão “Git”
O “Git” é um sistema de versionamento de aquivos, projetos, etc.
A playlist do canal RB Tech no YouTube é extremamente recomendada para compreensão do Git:
(https://youtu.be/WVLhm1AMeYE?list=PLInBAd9OZCzzHBJjLFZzRl6DgUmOeG3H0):
Intalação do Git
$ sudo apt-get install git
Configuração do Git
Informar nome de usuário e e-mail:
$ git config --global user.name "Seu Nome"
$ git config --global user.email "seunome@subdominio.dominio"
$ git config --list
GitHub - (github.com)
O GitHub é uma espécie de rede social de programadores no qual podemos disponibilizar nossos projetos, receber contribuições e também contribuir com outros projetos. Basta criar uma conta e depois adicionar as chaves ssh do computador no perfil do GitHub.
Gerar Chaves ssh - Acessso ao GitHub
(https://help.github.com/articles/generating-ssh-keys/)
$ ls -la .ssh/
$ ssh-keygen -t rsa -b 4096 -C "seuemail@example.com"
$ eval "$(ssh-agent -s)"
$ ssh-add ~/.ssh/id_rsa
$ ls -la .ssh/
Instalar o “xclip” que copia textos para área de transferência:
$ sudo apt-get install xclip
$ xclip -sel clip < ~/.ssh/id_rsa.pub
Por fim, adicionar a chave no GitHub, testar a conexão:
$ ssh -vT git@github.com
```

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

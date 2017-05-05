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

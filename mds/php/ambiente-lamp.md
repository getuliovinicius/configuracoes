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

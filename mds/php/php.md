Ambiente PHP
============

_Versão 1 - atualizada em 08/05/2017_

-----

_PHP_ é uma linguagem de _scripts_ de propósito geral que é especialmente adequado para desenvolvimento web. _PHP_ é um acrônimo para "Pré Processor HiperText", mas e daí ;)

## Instalação do PHP

**Procedimento para instalar o _PHP_ nos sistemas operacionais _Debian, Ubuntu e Linux Mint_:**

Antes antes de realizar a instalação do _PHP_ é recomendável adicionar um repositório que possibilite a atualização e o uso da última versão da linguagem, o que não costuma ser fácil especialmente no _Debian_.
Sendo assim, podemos adicionar os repositórios do site [https://www.dotdeb.org](https://www.dotdeb.org) ao arquivo `sources.list`, vamos lá:

```bash
$ sudo vim /etc/apt/sources.list
```

Copie o conteúdo abaixo para o final da lista

```
# Dotdeb
deb http://packages.dotdeb.org jessie all
deb-src http://packages.dotdeb.org jessie all
```

Na sequência instale a chave do repositório:

```bash
$ wget https://www.dotdeb.org/dotdeb.gpg
$ sudo apt-key add dotdeb.gpg
```

Por fim, quanto a adicão dos repositórios do site _dotdeb.org_, podemos atualizar a lista de pacotes disponíveis para instalação:

```bash
$ sudo apt update
```

Partindo para a instalação, o comando abaixo vai instalar o básico do PHP:

```bash
$ sudo apt install php7.0
```

!!! note "Info"
	**Pacotes instalados:** _libapache2-mod-php7.0 php-common php7.0 php7.0-cli php7.0-common php7.0-json php7.0-opcache php7.0-readline_

	**Observação:** A lista de pacotes instalados não deveria incluir o pacote _libapache2-mod-php7.0_, ocorre que eu já havia instalado o _Apache_ antes e talvez por essa razão tal pacote tenha sido instalado neste momento. Se isso ocorrer não há necessidade de seguir os passos descritos em **"Instalação do módulo PHP para o Apache"**.

Para checar a instalação do PHP execute o comando:

```bash
$ php --version
```
## Instalação do módulo PHP para o Apache

!!! note "Info"
	Neste exemplo irei utilizar o _Apache_ como servidor web, mas o _PHP_ pode ser utilizado com outros servidores web.
	Caso não tenha o _Apache_ instalado siga as instruções no tutorial: [Servidor Web Apache2](../servidores-web/apache.md).

Vamos instalar o módulo do apache _libapache2-mod-php_:

```bash
$ sudo apt-get install libapache2-mod-php7.0
```

## VirtualHost para o ambiente PHP

!!! note "Info"
	Totalmente opcional.

Para testar o funcionamento vamos criar um _VirtualHost_, ou simplesmente _vHost_, que sirva para checar o ambiente PHP e todas as suas características. Começaremos com a estrutura de diretórios para projetos de páginas dinâmicas em PHP.

```bash
$ sudo mkdir -p /srv/www/php/ambiente-php.local
$ cd /srv/www/php/ambiente-php.local
```

Crie um arquivo `info.php` dentro do diretório recém criado:

```bash
$ sudo vim index.php
```

Copie o conteúdo abaixo para o arquivo:

```php
<?php
phpinfo();
?>
```

Alterar o proprietário dos diretórios e arquivos em `/srv/www` para o usuário e grupo `www-data`:

```bash
$ sudo chown www-data\: /srv/www -R
```

Na sequência é preciso configurar o _vhost_.

```bash
$ cd /etc/apache2/sites-available
```

Criar um arquivo chamado `ambiente-php.local.conf`:

```bash
$ sudo vim /etc/apache2/sites-available/ambiente-php.local.conf
```

Copie o conteúdo abaixo para o arquivo:

```apacheconf
<VirtualHost *:80>
	DocumentRoot /srv/www/php/ambiente-php.local
	<Directory /srv/www/php/ambiente-php.local/>
		Options Indexes FollowSymLinks
		AllowOverride All
		Order allow, deny
		allow from all
	</Directory>
	ServerName ambiente-php.local
	ServerAdmin root@ambiente-php.local
	ErrorLog /var/log/apache2/ambiente-php.local_error.log
	CustomLog /var/log/apache2/ambiente-php.local_access.log common
</VirtualHost>
```

O novo _vHost_ deve ser habilitado usando o comando `a2ensite`, e o Apache reiniciado para persistir as mudança.

```bash
$ sudo a2ensite ambiente-php.local.conf
$ sudo systemctl restart apache2.service
```

E adicionar a entrada para o _vHost_ no arquivo `/etc/hosts`:

```bash
$ sudo vim /etc/hosts
```

Adicione a linha abaixo após as entradas criadas anteriormente para APP1 e APP2.

```rc
127.0.0.1	ambiente-php.local
```

Acesse os endereços [http://ambiente-php.local](http://ambiente-php.local) para verificar toda a configuração do PHP.

## Extensões do PHP

!!! attention "Atenção"
	As extensões do PHP devem ser instaladas e configuradas apenas para satisfazer as necessidades de aplicações que se queira usar. Não devem ser instalados módulos que não serão utilizados por nenhuma aplicação. As extensões que serão instaladas na sequência, são exigências para instalar aplicações como Phpmyadmin, MediaWiki, Drupal e também para desenvolver utilizando os Frameworks: Laravel e CodeIgniter por exemplo.

Comando para instalar as extensões _gd, mbstring, mcrypt, mysql, intl, xml_:

```bash
$ sudo apt install php7.0-gd php7.0-mbstring php7.0-mcrypt php7.0-mysql php7.0-intl php7.0-xml
```
!!! note "Info"
	**Pacotes instalados:** _libmcrypt4 php7.0-gd php7.0-intl php7.0-mbstring php7.0-mcrypt php7.0-mysql php7.0-xml_

Extensão _php7.0-curl_ usada para conectar _webservices_.

```bash
$ sudo apt install php7.0-curl
```

Gerenciador de dependências "PEAR": (opcional)

```bash
$ sudo apt-get install php-pear
```

Após instalar as extensões do PHP que julgar necessárias, é preciso reiniciar o Apache:

```bash
$ sudo systemctl restart apache2.service
```

Para listar os módulos do PHP instalados e habilitados, rode o comando:

```bash
$ php -m
```

## Gerenciador de dependências Composer

O _Composer_ pode ser instalado globalmente ou apenas no escopo do projeto que está em desenvolvimento. Siga as instruções abaixo para instalar globalmente, as mesmas podem podem ser vistas no site do gerenciador [https://getcomposer.org/](https://getcomposer.org/).

```bash
$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
$ php -r "if (hash_file('SHA384', 'composer-setup.php') === '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be6190fa0355160742ab2e1c88d40d5be660b410') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
$ php composer-setup.php
$ php -r "unlink('composer-setup.php');"
$ sudo mv composer.phar /usr/local/bin/composer
```

!!! attention "Atenção"
	A chave '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be
	6190fa0355160742ab2e1c88d40d5be660b410' pode não ser a mesma quando em uma instalação futura, é preciso conferir em [https://getcomposer.org/download/](https://getcomposer.org/download/).

Para verificar a versão do composer:

```bash
$ composer -V
```

## Configurar diretórios de desenvolvimento

No Linux o único diretório com permissão de escrita para o usuário padrão, o usuário da instalação do sistema operacional, é o diretório _home_ de sua conta: `/home/nome_usuario`.
Por essa razão não dá pra editar o código PHP no local que instalamos as aplicações, ou seja, o diretório `/srv/www/php`. Sendo assim vamos mudar as configurações de forma sutil para termos acesso de leitura e escrita aos arquivos de código fonte das aplicações.

!!! note "info"
	Em outros tutoriais irei demonstrar a instalação de aplicações como: _Drupal, Phpmyadmin, MediaWiki e WordPress_, tais aplicações não necessitam de alterações no código, portanto serão sempre instaladas no diretório `/srv/www/php`. A configuração mostrada até agora reflete de forma fidedigna as configurações ideais para instalação de aplicações PHP em servidores de produção. Daqui em diante veremos como transformar a instalação em algo próprio para desenvolvimento.

O primeiro passo é criar um diretório no diretório _home_ do usuário padrão, criado durante a instalação do sistema operacional Linux, por exemplo `/home/nome_usuario/DEV_WEB/PHP/VHOSTS`.

```bash
$ mkdir -p ~/DEV_WEB/PHP/VHOSTS
```

!!! note "info"
	O `~` (til) é um curinga que substitui o diretório _home_ no momento de digitar comandos no prompt.

Na pasta _VHOSTS_ iremos criar propriamente nossas aplicações, portanto será ali que iremos colocar os diretórios _.local_, por exemplo:

```bash
$ cd ~/DEV_WEB/PHP/VHOSTS
$ mkdir projeto1.local
```

Agora vamos criar um link simbólico para o diretório _projeto1.local_ dentro do diretório reconhecido pelo Apache `/srv/www/php/`:

```bash
$ cd /srv/www/php/
$ sudo ln -s ~/DEV_WEB/PHP/VHOSTS/projeto1.local .
$ sudo chown -R www-data\: projeto1.local
```

Acessamos o diretório de desenvolvimento criado na _home_ do usuário padrão:

```bash
$ cd ~/DEV_WEB/PHP/VHOSTS/
```
Então passamos para a mudança sutil que nos permitirá o acesso com leitura e escrita aos códigos fonte de aplicações desenvolvidas em PHP.
Alteramos o usuário dono, mas não o grupo, do diretório `~/DEV_WEB/PHP/VHOSTS/projeto1.local` para o usuário padrão:

```bash
$ sudo chown -R seu_uruario:www-data projeto1.local/
```

De agora em diante é possível editar projetos, códigos de aplicações desenvolvidas em PHP, com editores de texto como o _Sublime Text_.
Sempre que for rodar uma aplicação desenvolvida por você mesmo repita os passos na sequência, substituindo _projetoX.local_ pelo nome que você deseja:

```bash
$ cd /srv/www/php/
$ sudo ln -s ~/DEV_WEB/PHP/VHOSTS/projetoX.local
$ sudo chown -R www-data\: projetoX.local
$ cd ~/DEV_WEB/PHP/VHOSTS
$ mkdir projetoX.local
$ sudo chown -R seu_uruario:www-data projetoX.local/
```

A configuração dos _vHosts_ e as entradas no arquivo `/etc/hosts` podem ser feitas conforme o exemplo em [VirtualHost para o ambiente PHP](php.md#virtualhost_para_o_ambiente_php).

!!! note "info"
	Acostume-se a lidar com permissões de arquivos, principalmente com os comandos `chown` e `chmod`, pois isso é fundamental no desenvolvimento e quem não sabe usar corretamente esses comandos faz cagada em servidores.
	Não é brincadeira. Saber usar o prompt de comando e conhecer o funcionamento de permissões e propriedades de arquivos e diretórios faz toda diferença.

### Scripts

Se você não se empolgou com isso, fica então esses dois _scripts_, um para criar um novo projeto e outro para remover um projeto. Basta copiar o código e salvar em um arquivo dentro de um diretório de sua preferência, eu aconselho o deixar em `~/DEV_WEB/PHP` e logo após dar permissão de execução.

**novo-projeto.sh**

```bash
$ cd ~/DEV_WEB/PHP
$ vim novo-projeto.sh
```

Copie o conteúdo abaixo para o arquivo:

```sh
#!/bin/bash
# RECEBE O USUARIO ATUAL
USER_DEV=$USER
# LE O NOME DO DOMINIO PARA O PROJETO
echo "Informe o domínio para o projeto (Ex.: projeto-01.local):"
read DOMINIO
if [ -e "/etc/apache2/sites-available/$DOMINIO.conf" ]; then
	echo "ERRO: O domínio já existe."
	exit 1
fi
# LE A LINGUAGEM DO PROJETO
#echo "Informe a linguagem principal de desenvolvimento do projeto (Ex.: php):"
#read LINGUAGEM
LINGUAGEM="php"
LINGUAGEM_DEV_DIR="PHP"
# CRIA O DIRETORIO DO PROJETO
mkdir -pv $HOME/DEV_WEB/$LINGUAGEM_DEV_DIR/VHOSTS/$DOMINIO/
# MUDAR PARA SUPERUSUARIO
echo ">>>"
echo "Para criar o vHost no Apache é preciso rodar o script como super usuário."
echo "Certifique-se que seu usuário ($USER_DEV) tem permissão para usar o comando sudo."
echo ">>>"
sudo -u root bash << EOF
# ALTERA A PROPRIEDADE DO GRUPO DO DIRETORIO DO PROJETO PARA O GRUPO DO APACHE
chown -v $USER_DEV:www-data $HOME/DEV_WEB/$LINGUAGEM_DEV_DIR/VHOSTS/$DOMINIO
# CRIA UM LINK ENTRE DO DIRETORIO DO PROJETO PARA O DIRETORIO DO VHOST
ln -s $HOME/DEV_WEB/$LINGUAGEM_DEV_DIR/VHOSTS/$DOMINIO /srv/www/$LINGUAGEM/$DOMINIO
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
echo "127.0.0.1	$DOMINIO" >> /etc/hosts
EOF
echo "acesse: http://$DOMINIO"
exit
```

Para dar permissão de execução execute o comando:

```bash
$ chmod +x novo-projeto.sh
```

Para chamar o _script_ na linha de comando digite:

```bash
$ ./novo-projeto.sh
```

**remover-projeto.sh**

```bash
$ cd ~/DEV_WEB
$ vim remover-projeto.sh
```

Copie o conteúdo abaixo para o arquivo:

```sh
#!/bin/bash
# RECEBE O USUARIO ATUAL
USER_DEV=$USER
# LE O NOME DO DOMINIO PARA O PROJETO
echo "Informe o projeto que quer remover (Ex.: projeto-01.local):"
read DOMINIO
if [ ! -e "/etc/apache2/sites-available/$DOMINIO.conf" ]; then
	echo "ERRO: O domínio não existe."
	exit 1
fi
# LE A LINGUAGEM DO PROJETO
#echo "Informe a linguagem principal de desenvolvimento do projeto (Ex.: php):"
#read LINGUAGEM
LINGUAGEM="php"
LINGUAGEM_DEV_DIR="PHP"
if [ ! -d "/srv/www/$LINGUAGEM/$DOMINIO" ]; then
	echo "ERRO: Link do projeto não existe."
	exit 1
fi
# MUDAR PARA SUPERUSUARIO
echo ">>>"
echo "Para remover o vHost no Apache é preciso rodar o script como super usuário."
echo "Certifique-se que seu usuário ($USER_DEV) tem permissão para usar o comando sudo."
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
rm -rfv $HOME/DEV_WEB/$LINGUAGEM_DEV_DIR/VHOSTS/$DOMINIO/
```

Para dar permissão de execução execute o comando:

```bash
$ chmod +x remover-projeto.sh
```

Para chamar o _script_ na linha de comando digite:

```bash
$ ./remover-projeto.sh
```

E isso é tudo sobre PHP no momento...

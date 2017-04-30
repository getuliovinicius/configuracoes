Mkdocs
======

O _[Mkdocs](http://www.mkdocs.org/)_ é uma ferramenta desenvolvida em _Python_, especialmente para desenvolvimento de documentação de software, com qual é possível geral sites HTML estáticos a partir de arquivos escritos com a linguagem de marcação de texto _Markdown_.

Comando para criar o diretório onde ficaram os arquivos fonte da documentação:

``` sh
$ mkdir -p ~/DEV_WEB/PHP/VHOSTS/projeto-x.local/docs

# neste exemplo o diretório onde seriam mantidos os códigos fontes
# da aplicação que está sendo desenvolvida seria:
# ~/DEV_WEB/PHP/VHOSTS/projeto-x.local
```

Comando para criar o _virtualenv_ com o auxílio da ferramenta _virtualenvwrapper_:

``` sh
# exemplo
$ mkvirtualenv docs_mkdocs
```
!!! attention "Atenção"
    Para executar o comando anterior é preciso ter o [Python - virtualenv](virtualenv.md) configurado.

Comando para sair do _virtualenv_:

``` sh
$ deactivate
```

Comando para voltar para _virtualenv_:

``` sh
# exemplo
$ workon docs_mkdocs
```

Comando para atualizar o cache do repositório:

``` sh
$ pip install --upgrade pip
```

Comandos para instalar o _Mkdocs_:

``` sh
$ pip install mkdocs
```

Comando para checar a instalação:

``` sh
$ mkdocs --version
mkdocs, version x.x.x
```

Comando para iniciar o projeto de documentação no diretório corrente:

``` sh
$ mkdocs new .
```

Mkdocs
======

O _[Mkdocs](http://www.mkdocs.org/)_ é uma ferramenta desenvolvida em _Python_, especialmente para desenvolvimento de documentação de software, com qual é possível geral sites HTML estáticos a partir de arquivos escritos com a linguagem de marcação de texto _Markdown_.

Comando para criar o diretório onde ficaram os arquivos fonte da documentação:

```bash
$ mkdir -p ~/DEV_WEB/PHP/VHOSTS/projeto-x.local/docs
```

!!! note "Info"
    Neste exemplo o diretório onde seriam mantidos os códigos fontes da aplicação que está sendo desenvolvida seria: `~/DEV_WEB/PHP/VHOSTS/projeto-x.local`

Comando para criar o _virtualenv_ com o auxílio da ferramenta _virtualenvwrapper_:

```bash
$ mkvirtualenv docs_mkdocs
```

!!! attention "Atenção"
    Para executar o comando anterior é preciso ter o [Python - virtualenv](virtualenv.md) configurado.

Comando para sair do _virtualenv_:

```bash
$ deactivate
```

Comando para voltar para _virtualenv_:

```bash
$ workon docs_mkdocs
```

Comando para atualizar o cache do repositório:

```bash
$ pip install --upgrade pip
```

Comandos para instalar o _Mkdocs_:

```bash
$ pip install mkdocs
```

Comando para checar a instalação:

```bash
$ mkdocs --version
mkdocs, version x.x.x
```

Comando para iniciar o projeto de documentação no diretório corrente:

```bash
$ mkdocs new .
```

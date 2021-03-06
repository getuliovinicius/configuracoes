Sublime Text
============

_Versão 1 - atualizada em 05/05/2017_

-----

Procedimento para instalação do _Sublime Text_ nos sistemas operacionais _Debian, Ubuntu, Linux Mint_:

Faça o download do pacote para Debian em [https://www.sublimetext.com/](https://www.sublimetext.com/)

Comandos para instalação do _Sublime Text_ - presumindo que o download foi feito no diretório padrão:

```bash
$ cd ~/Downloads
$ sudo dpkg -i sublime-text\_build-3126\_amd64.deb
```

!!! attention "Atenção"
	Substitua "sublime-text\_build-3126\_amd64.deb" pelo nome do arquivo que baixou.

## Plugins

Segue uma lista com bons plugins para o _Sublime Text_:

+ Package Control
+ Emmet
+ DocBlockr
+ AdvancedNewFile
+ BracketHighlighter 
+ AutoFileName
+ HTML5
+ CSS Snippets
+ CSS Extended Completions
+ Alignment
+ Base​16 Color Schemes
+ ColorHighlighter
+ Apache​Conf.tm​Language
+ Keymaps
+ Git
+ GitGutter

## Configurações do usuário

Abaixo um exemplo de configuração do _Sublime Text_ editando o arquivo `Preferences.sublime-settings -- User` acessado a partir do menu `Preferences > Settings`:

```json
{
	"font_size": 11,
	"fade_fold_buttons": false,
	"line_padding_top": 1,
	"line_padding_bottom": 1,
	"highlight_line": true,
	"highlight_modified_tabs": true,
	"bold_folder_labels": true,
	"ignored_packages":
	[
		"Vintage"
	]
}
```



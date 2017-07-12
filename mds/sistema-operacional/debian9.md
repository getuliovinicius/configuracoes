Debian 9 - Strech
=================

_Versão 1 - atualizada em 11/07/2017_

-----

# Objetivo

Este tutorial objetiva demonstrar a instalação e configuração do sistema operacional _Debian9 - Strech_ com um ambiente gráfico enxuto, ou seja, apenas com programas básicos ocupando o mínimo de recursos do computador. Tal instalação pode ser personalizada de acordo com as necessidades do usuário.

# Preparação do ambiente no _VirtualBox_

Para este exemplo de instalação e configuração de um sistema operacional _Debian9_ foi criada uma máquina virtual utilizado o software de virtualização _VirtualBox_ com as seguintes especificações:  

## Descrição e Tipo

+ **Nome:** DebianVM-02-modelo-02
+ **Tipo:** Linux
+ **Versão:** Debian (64-bit)

![Imagem 1](imgs/debian9-1.png)

!!! note "Nota"
    O _VirtualBox_ possui a funcionalidade de criação de clones das máquinas virtuais. Essa funcionalidade é especialmente adequada para situações em que se planeja utilizar o mesmo sistema operacional para configurar ambientes com diversas finalidades, por exemplo: ambiente de programação em PHP, ambiente de programação em Java, ambiente de edição de imagens, ambiente de teste de redes ou qualquer outro ambiente. Basta clonar uma máquina virtual com uma instalação básica do sistema operacional que pretende utilizar e posteriormente configurá-lo de acordo com as necessidades do ambiente desejado.
    Este tipo de abordagem evita o acumulo de programas instalados no sistema operacional, tornando-o mais eficiente em termos de utilização de recursos, ademais é possível manter muitas máquinas virtuais em um computador.

Após instalação básica do _Debian9_ a máquina virtual foi clonada, para que permaneça como modelo para outras personalizações de ambiente.

## Hardware

+ Foram disponibilizados até 4GB de Memória RAM

![Imagem 2](imgs/debian9-2.png)

+ Foi criado um disco virtual com até 40GB de espaço de armazenamento

![Imagem 3](imgs/debian9-3.png)

![Imagem 4](imgs/debian9-4.png)

![Imagem 5](imgs/debian9-5.png)

+ Foram disponibilizados 2 Processadores(CPUs)

![Imagem 7](imgs/debian9-7.png)

+ A memória dedicada a vídeo foi configurada para 128MB e a Aceleração 3D foi habilitada

![Imagem 8](imgs/debian9-8.png)

+ A imagem de instalação do Debian9 foi carregada

![Imagem 9](imgs/debian9-9.png)

+ Foi definida a utilização de uma placa de rede em modo Bridge

![Imagem 10](imgs/debian9-10.png)

O restante das opções do da máquina foram mantidas como padrão do _VirtualBox_.

# Instalação Básica

+ Foi escolhida a opção de instalação em modo texto

![Imagem 13](imgs/debian9-13.png)

+ Foi escolhido o idioma de instalação **Português do Brasil**

![Imagem 14](imgs/debian9-14.png)

+ Foi escolhido o país **Brasil**

![Imagem 15](imgs/debian9-15.png)

+ Foi escolhido o layout de teclado **Português do Brasil**

![Imagem 16](imgs/debian9-16.png)

+ Foi definido o nome de máquina **DebianVM-02-modelo-02**

![Imagem 17](imgs/debian9-17.png)

!!! note "Nota"
    Posteriormente o nome de máquina foi alterado na máquina clonada.

+ Foi definido o nome de domínio **local**

![Imagem 18](imgs/debian9-18.png)

+ A senha do super-usuário, **root**, foi deixada em branco

![Imagem 19](imgs/debian9-19.png)

![Imagem 20](imgs/debian9-20.png)

!!! note "Nota"
    A senha de super-usuário foi deixada em branco propositalmente para que o comando sudo seja configurado automaticamente durante a instalação.

# Clone

![Imagem 11](imgs/debian9-11.png)

![Imagem 11](imgs/debian9-12.png)

# Pós clone

+ Connfiguração

# servidor_residencial

## Sumário

1. Apresentação
    1.  [Introdução](#Introdução)
    2.  [Por que faser um servidor físico?](#por_que_fazer)
    3.  [Requisitos](#Requisitos) 
2. Configurando o Servidor
    1. [Instalando o Linux](#Instalando_o_Linux)
        * [Pendriver bootavel](#pendrive_boot)
        * [Usando SSH](#ssh)
    2. [Verificando a Internet](#Verificando_a_Internet)
    3. [Atualisando o Sistema](#Atualizando_o_sistema)
    4. [Instalando o SSH](#Instalando_o_SSH)
    5. [Instalando o SAMBA](#Instalando_o_SAMBA)
    6. [Instalando o PostgreeSQL](#Instalando_o_PostgreeSQL)
   

## 1 Apresentação

<a id="Introdução"></a>

## 1 I Introdução

<a id="por_que_fazer"></a>

## 1 II Por que faser um servidor físico?

<a id="Requisitos"></a>

## 1 III Requisitos

#### Sistema Operacional

As distribuições **Linux** maiis comuns em servidores são Ubuntu, Debian, SUSE, Fedora, Red Hat, CentOS

## 2 Configurando o Servidor

<a id="Instalando_o_Linux"></a>

### 2 I Instalando o Linux



<a id="pendrive_boot"></a>

#### Pendriver bootavel

Este início é igual para qualquer distribuição linux, Você vai presisar baixar a imagem ISO do site da Ubuntu e criar um pen drive com esta imagem usando um dos programas abaixo:

* Rufus (Acho que este é o único que só possui verção para Windows);
* Etcher (Este, assim como o acima, nunca usei);
* Ventoy (permite colocar várias imagens ISO em um único pendrive apenas arrastando o arquivo para ele);
* YUMI  (foi o primeiro que usei e é muito fácil de usar).

. Feito isto, ligue o computador com o pen drive já conectado na porta USB e entre na tela de opção de boot, geralmente apertando a tecla *F8*, selecione o pen drive que está a imagem ISO e confirme para abrir o assistente de instalação. É recomendado usar uma verção própria para servidores. o Ubuntu Server possui um assistente de instalação diferente das distros voltadas para uso pessoal. Como não possui interface gráfica a instalação é feita toda em modo texto porém o que é preciso fazer aparece em etapas na tela onde deve-se apenas escolher as opções usando as setas, marcar opções usando a tecla espaço e confirmar precionando enter. Ao final do processo seu sistema estará instalado. Caso tenha difivuldades pode optar por uma versão com interface gráfica mas é extremamente recomendado que seja uma versão LTS.

<a id="ssh"></a>

#### Usando SSH

O Ubuntu server já dá a opção de instalar o programa SSH que permite acesso seguro ao computador remotamente. Caso não tenha instalado use o comando `sudo apt install ssh`.

A chave SSH usa o IP para se conectar. Durante a instalação esta informação é mostrada na etapa que configura a internet. caso não tenha anotado use o comando `ifconfig` . Será mostrado uma sequencia de números iniciando com 192.168 e seguida de mais dois grupos de dígitos separados por ponto.

Com estas informações já é possivel acessar o servidor usando o SSH mas é possivel seguir com a configuração diretamente no servidor ou então pode  desligá-lo; colocá-lo ao lado do seu roteador de internet; conectá-los usando um cabo de rede e ligar o PC novamente sem monitor ou teclado.

De um outro computador, abra o terminal se for um Linux e escreva *sudo ssh nome_do_usuário@ip*, caso seja Windows, abra o CMD ou o Powershell e escreva a mesma coisa mas sem usar o sudo.

~~~shell
ph@A320:~$ sudo ssh operador@192.168.0.78
The authenticity of host '192.168.0.78 (192.168.0.78)' can't be established.
ECDSA key fingerprint is SHA256:+lySxZ6u2RtnXtoQ9Oo1rHZ5c/feCiaQcuO8iNgLqj8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.0.78' (ECDSA) to the list of known hosts.
operador@192.168.0.78's password: 
Welcome to Ubuntu 22.04 LTS (GNU/Linux 5.15.0-43-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of dom 07 ago 2022 01:40:51 UTC

  System load:             0.0
  Usage of /:              14.0% of 48.91GB
  Memory usage:            3%
  Swap usage:              0%
  Processes:               125
  Users logged in:         0
  IPv4 address for enp3s0: 192.168.0.78
  IPv6 address for enp3s0: 2804:14d:5c9d:8162::1f46
  IPv6 address for enp3s0: 2804:14d:5c9d:8162:4a5b:39ff:fece:f1c5

 * Super-optimized for small spaces - read how we shrank the memory
   footprint of MicroK8s to make it the smallest full K8s around.

   https://ubuntu.com/blog/microk8s-memory-optimisation

34 updates can be applied immediately.
To see these additional updates run: apt list --upgradable


Last login: Sat Aug  6 17:33:09 2022
operador@siscasa:~$ 
~~~

 


<a id="Verificando_a_Internet"></a>

### 2 II Verificando a Internet

<a id="Atualizando_o_sistema"></a>

### 2 III Atualizando o sistema

<a id="Instalando_o_SSH"></a>

### 2 IV Instalando o SSH

<a id="Instalando_o_SAMBA"></a>

### 2 V Instalando o SAMBA

<a id="Instalando_o_PostgreeSQL"></a>

### 2 VI Instalando o PostgreeSQL

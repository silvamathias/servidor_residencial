# servidor_residencial

## Sumário

1. Apresentação
    1.  [Introdução](#Introdução)
    2.  [Por que faser um servidor físico?](#por_que_fazer)
    3.  [Requisitos](#Requisitos) 
2. Configurando o Servidor
    1. [Instalando o Linux](#Instalando_o_Linux)
        * [Pen drive bootavel](#pendrive_boot)
        * [Usando SSH](#ssh)
    2. [Atualisando o Sistema](#Atualizando_o_sistema)
    3. [Configurando IP fixo](#ip_fixo)
    4. [Instalando o SAMBA](#Instalando_o_SAMBA)
    5. [Instalando o PostgreeSQL](#Instalando_o_PostgreeSQL)
   

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

#### Pen drive bootavel

Este início é igual para qualquer distribuição linux, Você vai presisar baixar a imagem ISO do site da Ubuntu e criar um pen drive com esta imagem usando um dos programas abaixo:

* Rufus (Acho que este é o único que só possui verção para Windows);
* Etcher (Este, assim como o acima, nunca usei);
* Ventoy (permite colocar várias imagens ISO em um único pendrive apenas arrastando o arquivo para ele);
* YUMI  (foi o primeiro que usei e é muito fácil de usar).

. Feito isto, ligue o computador com o pen drive já conectado na porta USB e entre na tela de opção de boot, geralmente apertando a tecla *F8*, selecione o pen drive que está a imagem ISO e confirme para abrir o assistente de instalação. É recomendado usar uma verção própria para servidores. o Ubuntu Server possui um assistente de instalação diferente das distros voltadas para uso pessoal. Como não possui interface gráfica a instalação é feita toda em modo texto porém o que é preciso fazer aparece em etapas na tela onde deve-se apenas escolher as opções usando as setas, marcar opções usando a tecla espaço e confirmar precionando enter. Ao final do processo seu sistema estará instalado. Caso tenha difivuldades pode optar por uma versão com interface gráfica mas é extremamente recomendado que seja uma versão LTS.

<a id="ssh"></a>

#### Usando SSH

O Ubuntu server já dá a opção de instalar o programa SSH que permite acesso seguro ao computador remotamente. Caso não tenha instalado use os comandos:

* `sudo apt install opensh-server` para instalar o programa; 
* `sudo service ssh start` para iniciar o serviço;
* `ps aux | grep ssh` para testar o programa.

A chave SSH usa o IP para se conectar. Durante a instalação esta informação é mostrada na etapa que configura a internet. caso não tenha anotado use o comando `ip a` . Será mostrado uma sequencia de números iniciando com 192.168 e seguida de mais dois grupos de dígitos separados por ponto.

~~~shell
operador@siscasa:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 48:5b:39:ce:f1:c5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.78/24 metric 100 brd 192.168.0.255 scope global dynamic enp3s0
       valid_lft 2564sec preferred_lft 2564sec
    inet6 2804:14d:5c20:9479:4a5b:39ff:fece:f1c5/64 scope global dynamic mngtmpaddr noprefixroute
       valid_lft 3600sec preferred_lft 3600sec
    inet6 fe80::4a5b:39ff:fece:f1c5/64 scope link
       valid_lft forever preferred_lft forever
~~~

No código acima é possível ver o retorno do comando `ip a` informando o ip do servidor como sendo **192.168.0.78**

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

 A linha `operador@siscasa:~$` indica que a partir de agora você está dentro do servidor onde o seu usuário é *operador* e o nome do servidor é *siscasa* que foram definidos na instalação do Ubuntu. Note também que nem sempre, ao digitar uma senha no terminal Linux, aparecerá '*' para cada tecla clicada. 


<a id="Atualizando_o_sistema"></a>

### 2 II Atualizando o sistema


`sudo -i`
`apt update`

~~~shell
operador@siscasa:~$ sudo -i
[sudo] password for operador: 
root@siscasa:~# apt update
~~~

`apt upgrade`

<a id="ip_fixo"></a>

### 2 III Configurando IP fixo

Voltando ao resultado do comando `ip a`, além do ip atual, é possível decobrir o nome da placa de rede que, neste exemplo é **enp3s0**
~~~shell
2: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 48:5b:39:ce:f1:c5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.78/24 metric 100 brd 192.168.0.255 scope global dynamic enp3s0
       valid_lft 2564sec preferred_lft 2564sec
    inet6 2804:14d:5c20:9479:4a5b:39ff:fece:f1c5/64 scope global dynamic mngtmpaddr noprefixroute
       valid_lft 3600sec preferred_lft 3600sec
    inet6 fe80::4a5b:39ff:fece:f1c5/64 scope link
       valid_lft forever preferred_lft forever
~~~

Use o comando **resolvectl status** para visualizar os servidores DNS e anote os IP's da linha **DNS Servers**.

~~~shell
root@siscasa:~# resolvectl status
Global
       Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
resolv.conf mode: stub

Link 2 (enp3s0)
    Current Scopes: DNS
         Protocols: +DefaultRoute +LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 181.213.132.2
       DNS Servers: 192.168.0.1 181.213.132.2 181.213.132.3 2804:14d:1:0:181:213:132:2
                    2804:14d:1:0:181:213:132:3
~~~

Vá até a pasta netplan com o comando **cd /etc/netplan** e procure o arquivo de configuração. Antes de remove-lo, copie por preucalção com outro nome para deixar de backup. No exemplo abaixo foi usado o `nano` para criar um novo  arquivo. A regra é que o nome começe com número e termine com **.yaml**.

~~~shell
root@siscasa:~# cd /etc/netplan
root@siscasa:/etc/netplan# ls
00-installer-config.yaml
root@siscasa:/etc/netplan# cp 00-installer-config.yaml  bk-00-installer-config.yaml
root@siscasa:/etc/netplan# rm 00-installer-config.yaml
root@siscasa:/etc/netplan# nano 01-config-rede.yaml
~~~

Copie o código abaixo e cole dentro do arquivo de configuração que acabou de criare depois atualize os campos abaixo:

* Altere **enp3s0** pelo nome da placa de rede de seu servidor;
* Coloque aqui o IP que deseja usar no servidor. Neste exemplo foi trocado **192.168.0.78/24** por **192.168.0.101/24**;
* Substitua **[192.168.0.1, 181.213.132.2]** pelos dois primeiros IP's DNS no padrão IPV4 que anotou na linha **DNS Servers**. Caso só tenha um, delete **, otherdomain** do item **search:** localizado na linha superior.

Provavelmente o item **via** não precisará ser atualizado pois é onde se informa o IP do roteador e a maioria dos roteadores domésticos usam como padrão **192.168.0.1**.
Neste arquivo a identação é importante para o funcionamento.

~~~shell
#configurando a interface enp3s0 com um IP estático
network:
    version: 2
    renderer: networkd
    ethernets:
        enp3s0:
            addresses:
                - 192.168.0.101/24
            nameservers:
                search: [mydomain, otherdomain]
                addresses: [192.168.0.1, 181.213.132.2]
            routes:
                - to: default
                  via: 192.168.0.1
~~~

Salve clicando **Ctrl + S** e saia do arquivo clicando **Ctrl + x**.
Teste a configuração com o comando `netplan try`. Caso tenha dado tudo certo a menságem abaixo será mostrada. Clique **ENTER** conforme solicitado e aplique a as alterações com o comando `netplan apply`.

~~~shell
root@siscasa:/etc/netplan# netplan try
Do you want to keep these settings?


Press ENTER before the timeout to accept the new configuration


Changes will revert in  98 seconds
Configuration accepted.
root@siscasa:/etc/netplan# netplan apply
~~~

<a id="Instalando_o_SAMBA"></a>

### 2 V Instalando o SAMBA

<a id="Instalando_o_PostgreeSQL"></a>

### 2 VI Instalando o PostgreeSQL

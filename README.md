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

#### Instalando a verção mais recente.

O código abaixo consta no site do [PostgreSql](https://www.postgresql.org/download/linux/ubuntu/). Funcionou copiando e colando no terminal linha a linha.

~~~shell
# Create the file repository configuration:
sudo sh -c 'echo "deb https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

# Import the repository signing key:
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

# Update the package lists:
sudo apt-get update

# Install the latest version of PostgreSQL.
# If you want a specific version, use 'postgresql-12' or similar instead of 'postgresql':
sudo apt-get -y install postgresql
~~~


#### Configurar o Firewall

~~~shell
#Ative o ufw
operador@siscasa:~$ sudo ufw enable
[sudo] password for operador: 
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup

#liberar o firewall para o postgre:
operador@siscasa:~$ sudo ufw allow postgresql
Rule added
Rule added (v6)

#reinicie o serviço postgre:
operador@siscasa:~$ sudo systemctl restart postgresql.service 
operador@siscasa:~$ 
~~~

#### Alterando a senha do Postgre.

Entre no Postgre com o comando `sudo -u postgres psql` e altere a senha usando `alter user postgres with encrypted password 'operador123456';`. Coloque sempre ***';'** no final para não apresentar erro. O comando **'\q'** sai do Postgre.
Depois crie um novo usuário e consigure sua senha.

~~~shell
sudo -u postgres psql
psql (16.0 (Ubuntu 16.0-1.pgdg22.04+1))
Type "help" for help.

postgres=# alter user postgres with encrypted password 'operador123456';
ALTER ROLE

# criando usuário novo
postgres=# create user operador createdb;
CREATE ROLE

# alterando a senha do usuário novo
postgres=# alter user operador with password '123456';
ALTER ROLE

# tornando a senha válida infinitamente
postgres=# alter user operador valid until 'infinity';
ALTER ROLE

# verificando se o usuário foi criado
postgres=# select * from pg_user;
~~~


#### Criando e acessando um banco de dados 

Só será possível criar um banco de dados usando o psql. Caso não tenha certesa do banco que deseja criar, siga o exemplo abaixo mas saiba que terá que acessar desta forma para criar outros banco.

~~~shell
# criando banco
postgres=# create database cidades;
CREATE DATABASE

# conectando ao banco
postgres=# \c cidades
You are now connected to database "cidades" as user "postgres".
cidades=#

# listando os bancos de dados existentes
cidades=# \l
                                                       List of databases
   Name    |  Owner   | Encoding | Locale Provider |   Collate   |    Ctype    | ICU Locale | ICU Rules |   Access privileges   
-----------+----------+----------+-----------------+-------------+-------------+------------+-----------+-----------------------
 cidades   | postgres | UTF8     | libc            | pt_PT.UTF-8 | pt_PT.UTF-8 |            |           | 
 postgres  | postgres | UTF8     | libc            | pt_PT.UTF-8 | pt_PT.UTF-8 |            |           | 
 template0 | postgres | UTF8     | libc            | pt_PT.UTF-8 | pt_PT.UTF-8 |            |           | =c/postgres          +
           |          |          |                 |             |             |            |           | postgres=CTc/postgres
 template1 | postgres | UTF8     | libc            | pt_PT.UTF-8 | pt_PT.UTF-8 |            |           | =c/postgres          +
           |          |          |                 |             |             |            |           | postgres=CTc/postgres
(4 rows)

cidades=# \q
~~~

Outros comando úteis:

**Comando**|**Descrição**
|:---:|:---|
\d|lista as tabelas do banco de dados
\dv|lista as views do banco de dados
\di|lista os índices do banco de dados
\db|lista as tablespaces
\l|lista os bancos de dados
\dg|lista as roles existentes (usuários ou grupos)
\conninfo|apresenta informações sobre a conexão atual
\h|lista os comandos SQL
\h comando|apresenta detalhes sobre o comando
\q|sai do **PostgreSql**

#### Configurando o acesso ao PostgreSql.

~~~shell
# Ir até a pasta com os arquivos para configuração. Perceba que a pasta **'16'** deve ser trocada caso sua versão não seja a 16 
operador@siscasa:~$ cd /etc/postgresql/16/main/

# liste os arquivos da pasta
operador@siscasa:/etc/postgresql/16/main$ ls
conf.d  environment  pg_ctl.conf  pg_hba.conf  pg_ident.conf  postgresql.conf  start.conf

# crie um backup dos arquivos postgresql.conf e pg_hba.conf 
operador@siscasa:/etc/postgresql/16/main$ sudo cp postgresql.conf bk_postgresql.conf 
operador@siscasa:/etc/postgresql/16/main$ sudo cp pg_hba.conf bk_pg_hba.conf 

# verifique se os backup's foram criados com sucesso
operador@siscasa:/etc/postgresql/16/main$ ls
bk_pg_hba.conf      conf.d       pg_ctl.conf  pg_ident.conf    start.conf
bk_postgresql.conf  environment  pg_hba.conf  postgresql.conf
~~~

Abra o arquivo **postgresql.conf** com o comando `sudo nano postgresql.conf` e procure pela linha:

`#listen_addresses = "localhost" # what IP address(es) to listen on`

Retire o `#` para descomentar a linha e mude o valor para `*`. O resultado final será:

`listen_addresses = "*" # what IP address(es) to listen on`


Agora abra o arquivo **pg_hba.conf** com o comando `sudo nano pg_hba.conf` e inclua uma linha no final do arquivo com a configuração desejada podendo variar de:

perfil mais permissivo, onde qualquer usuário de qualquer IP poderá se conectar a qualquer database.

`host all all 0.0.0.0/0 trust`

Ao perfíl mais restritivo, onde o usuário usuariodobanco poderá conectar-se apenas ao banco *bancodedados* apenas se estiver dentro da rede *192.168.0.0*.

`host bancodedados usuariodobanco 192.168.0.0/32 md5`

Tenha em mente que os parâmetro **trust** e **md5** são:

trust => não solicita senha 

md5 => solicita senha

Para este exemplo foi incluido uma linha de comentário para explicitar que a configuração abaixo foi criada pelo usuário. O resultado final ficou conforme abaixo:

~~~shel
# configuracao manual
host    all        all        0.0.0.0/0    md5
~~~

Depois de salvar e sair de cada um dos arquivos, reinicie o **PostgreSql** com o comando `sudo systemctl restart postgresql.service` 

#### Como acessar o banco remotamente?

Até o momento só o **PostgreSql** só foi acessado através do psql uma vez que esteja logado no servidor por uma conexão remota com ssh ou fisicamente com um acesso direto ao servidor fisicamente. É recomendado que se use o [DBeaver](https://dbeaver.io/download/) para isto. 

*DBeaver* é um cliente SQL multiplataforma, com versões para Linux, Windows e Mac, possibilitando o acesso e o gerenciamento de diversos bancos de dados, entre eles o **PostgreSql**. Basta realizar a instalação, abrir o programa e criar uma conexão nova informando **o host, o banco de dados, o usuário e a senha**, conforme abaixo. O campo *URL* será atualizado automaticcamente.

![Exemplo de uso e configuração da conexão usando o *DBeaver*](https://github.com/silvamathias/servidor_residencial/blob/main/media/conexao_postgre.png)

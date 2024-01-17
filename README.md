# servidor_residencial

## Sumário

1. Apresentação
    * 1.1 [Introdução](#introducao)
    * 1.2 [Por que faser um servidor físico?](#por_que_fazer)
    * 1.3 [Requisitos](#requisitos)
        * 1.3.1 [Sistema Operacional](#sys_operacional)
    * 1.4 [Cuidados com a segurança](#seguranca)

2. Instalando o Linux
    * 2.1 [Criando Pen drive bootavel e instalação](#criando_pendrive)
    * 2.2 [Atualizando o sistema](#atu_sys)
    * 2.3 [Comandos básicos](#comandos_basicos)

3. Configurando o servidor
   * 3.1 [Configurando o Firewall](#configurando_firewall)
   * 3.2 [Configurando IP estático](#configurando_ip_fixo)
   * 3.3 [Usando SSH](#ssh)
   * 3.4 [Usuários e grupos](#usuario_grupo)
      * 3.4.1 [Gerenciando usuários](#gerencia_usuario)
      * 3.4.2 [Gerenciando grupos](#gerencia_grupo)

   * 3.5 [Configurações opcionais](#configuracoes_especiais)
      * 3.5.1 [Montando partição automaticamente ao ligar o servidor](#montando_particao)
      * 3.5.2 [Configurando a tampa do Notebook](#tampa_notebook)

4. Atribuindo funcionalidades
   * 4.1 [Instalando o SAMBA](#samba)
      * 4.1.1 [Acessando de uma máquina Windows](#acesso_samba_windows)
      * 4.1.2 [Acessando de uma máquina Linux](#acesso_samba_linux)
   * 4.2 [Instalando o PostgreeSQL](#instalando_postgre)
      * 4.2.1 [Instalando a verção mais recente](#Instalando_mais_atual)
      * 4.2.2 [Habilitando a porta  do Firewall](#habilitando_firewall)
      * 4.2.3 [Alterando a senha do Postgre](#alterando_senha_postgre)
      * 4.2.4 [Criando e acessando um banco de dados](#criando_e_acessando_bd)
      * 4.2.5 [Configurando o acesso ao PostgreSql](#configurando_acesso_postgre)
      * 4.2.6 [Como acessar o banco remotamente](#acesso_remoto_postgre)

5. Considerações finais
   * 5.1 [Aprofundando conhecimento](#aprofundando_conhecimento)


## 1 Apresentação

<a id="introducao"></a>

### 1.1 Introdução

<a id="por_que_fazer"></a>

### 1.2 Por que fazer um servidor físico?

<a id="requisitos"></a>

### 1.3 Requisitos

<a id="sys_operacional"></a>

### 1.3.1 Sistema Operacional

As distribuições **Linux** mais comuns em servidores são Debian, Ubuntu, SUSE, Red Hat Enterprise Linux, CentOS e Fedora. A **Ubuntu** é mantida pela empresa Canonical. É uma distro (abreviação de distribuição) derivada do Debian e a maior diferença entre elas é que o Debian restringe o uso de programas proprietários, o que também inclui drivers, já a **Ubuntu** deixa esta decisão por conta do usuário final. A Distro Red Hat é mantida pela empresa de mesmo nome. Seu foco principal é o mercado corporativo, vendendo serviços de suporte e manutenão para as empresas que queiram criar seus hambientes usando linux. Por conta disso é difícil uma pessoa física usar seu sistema, contando apenas com um prazo de 30 dias para testar a distro antes de ter que pagar pelo cerviço. A opção mais usada são as distros derivadas do Red Hat, CentOS e Fedora, que são mantidas pela comunidade porém contam com o apoio da Red Hat.

Para este projeto foi escolhida a distro Ubuntu, muito por conta de sua popularidade, o que a faz uma distro de fácil acesso a materias para consulta na internet, além do site da propria Canonical.

<a id="seguranca"></a>

### 1.4 Cuidados com a segurança

Este tutorial tem o objetivo didático, aprender mais sobre Linux com uma abordagem mais profissional, porém com o intúito de ser acessível a pessoas de todos os níveis de conhecimanto. Tendo isto em vista a prioridade é que quem a use consiga fazer um servidor funcionar mesmo com brechas de segurança, o que pode facilitar invasões ao sistema. A idéia é que ao ver que o servidor está funcionando, o leitor se sinta motivado e sinta que já aprendeu algo. Se de fato queira mais segurança ou busque aprofundar mais nos tópicos bastará seguir as insugestões das **Notas de Segurança** toda vez que apareça uma em algum tópico.
  

## 2 Instalando o Linux

<a id="criando_pendrive"></a>

### 2.1 Criando Pen drive bootavel e instalação

Este início é igual para qualquer distribuição linux, Acesse o site da distro escolhida e baixe a imagem ISO do sistema. Para baixar a distro usada neste projeto basta ir até o site da [Ubuntu](https://ubuntu.com/download/server). Caso queira usar a mesma versão, procure por *Ubuntu 22.04.3 LTS*. Já com a imagem ISO em sua máquina, crie um pen drive com esta imagem usando um dos programas abaixo:

* Rufus;
* Etcher;
* Ventoy (permite colocar várias imagens ISO em um único pendrive apenas arrastando o arquivo para ele);
* YUMI.

. Feito isto, ligue o computador com o pen drive já conectado na porta USB e entre na tela de opção de boot, geralmente apertando a tecla *F8*, selecione o pen drive que está a imagem ISO e confirme para abrir o assistente de instalação. É recomendado usar uma verção própria para servidores. o **Ubuntu** Server possui um assistente de instalação diferente das distros voltadas para uso pessoal. Como não possui interface gráfica a instalação é feita toda em modo texto porém o que é preciso fazer aparece em etapas na tela onde deve-se apenas escolher as opções usando as setas, marcar opções usando a tecla espaço e confirmar precionando enter. Ao final do processo seu sistema estará instalado. Caso tenha difivuldades pode optar por uma versão com interface gráfica mas é extremamente recomendado que seja uma versão LTS.

<a id="atu_sys"></a>

### 2.2 Atualizando o sistema

Entre no sistema usando o Usuário e a senha cadastrados durante a instalação. Este usuário criado durante a instalação estará configurado por padrão no sistema como um usuário *sudo*. Isto significa que este usuário poderá ter privilégios do administrador do hambiente através do comando *sudo* antes do comando que deseja executar. O primeiro uso irá solicitar a senha de seu usuário e a senha não será mais necessáris por alguns minutos. Outra opção e usar o comando `su`, `sudo -i` ou `sudo su`, sendo o primeiro o mais indicado. Com esses comandos você ficará logado no sistema como usuário administratido. o símbolo `$` será trocado por `#` e não será solicitado senhas até que encerre o acesso com o comando `exit`. Ao longo desteartigo será usado aamgbos os casos então observe que toda vez que o símbolo "$" está presente o *sudo* foi usado para determinados comandos e quando estiver mostrando "#", o *sudo* não foi usado.

Toda distro Linux possui um *gerenciador de pacotes*. No **Ubuntu** é o *apt*. Com ele você ira atualizar primeiro a lista de repositórios e depois os programas instalados. Para tal o comando *sudo* deverá ser usado em conjunto. Use `sudo apt update`, clique em *Enter* para executar, depois `sudo apt upgrade` e clique em *Enter* novamente. Este comando irá solicitar que digite *y ou s* para confirmar e *n* para negar a execução do comando. caso queira fazer tudo isto em uma única linha digite:

~~~shell
$ sudo apt update && sudo apt upgrade -y
~~~

onde **&&** une os dois comandos para serem executados em sequência e **-y** autoriza automaticamente caso haja a necessidade de validarção 

<a id="comandos_basicos"></a>

### 2.3 Comandos Básicos

O Linux é um sistema que funciona por *linha de comando* e que é possivem instalar interfaces gráficas para facilitar o uso em computadores pessoais. A sigla **CLI** (Command Line Interface) é usada para se referir a estes comandos usados no terminal ou em sistemas que não possuem interfaces gráficas como é o caso dos servidores. Caso seja novo no **Linux**, abaixo segue uma lista dos principais comando usados neste projeto. O próprio manual servirá como exemplo de uso. Os comandos `man` e `help`te ajudarão a descobrir mais funcionalidades deles. Usando o comando `ls` como exemplo:

~~~shell
$ man ls
$ ls --help
~~~

Realizar uma pesquisa na internet por eles também será útil

**Comando**|**Descrição**
|:---:|:---|
--help|mostra uma lista de comando mais resumida que o comando man
apt (apt-get)|gerenciador de pacote do Debian e suas distros derivadas
btop|possui o mesmo objetivo que o *htop* mas com um visual diferente
cat|mostra o conteudo de um arquivo texto
cd|navega pelos diretórios do sistema
chmod|altera as permissões de arquivos ou pastas, segregando por dono, grupo e visitantes 
chown|altera o dono e o grupo de um arquivo ou pasta
clear|limpa a tela do terminal. As teclas *"Ctrl l"* também funciona da mesma forma
cp|copia um arquivo
grep|realiza um filtro no comando anterior
htop|cli gerenciador de processos. Informa o uso dos recursos por cada programa e permite encerrar sua execução
inxi|Mostra as informações do sistema, tanto de software quanto de hardware
ip a|mostra os IP’s dos dispositivos de rede
ls|lista os itens de um diretório
lsblk|mostra informações sobre os dispositivos de armazenamento (HD, SSD, Pen Drive)
man |mostra o manual do comando desejado
mount|monta uma partição, seja pendrive, CD, HD, etc
mkdir|cria um diretório
nano|abre um arquivo de texto com o editor nano
neofetch|é uma verção simplificada do comando *inxi* e que mostra o logo da distro usada
resolved|comando vinculado ao Systemd que gerencia os serviços DNS
rm|remove um arquivo
systemctl|comando referente ao Systemd, gerenciador de serviços e sistema

Alguns destes comando ficarão em uso até que seja encerrado pelo usuário. Geralmente a letra *q* de *quit* é usada para isto. Caso não funcione, precione as téclas *"Ctrl c"* para encerrar qualquer processo que esteja sendo executado pelo terminal.

Caso um desses comandos acima não esteja instalado irá retornar uma mensagem de erro, podendo até sugerir sua instalação informando o comando para isto.

Caso isto ocorra com o *btop* por exemplo, basta usar o comando `sudo apt install btop` para instalá-lo

## 3 Configurando o servidor

Este tópico trata sobre a configuração básica do servidor, alterações que devem ser feitas para o bom uso do mesmo independente do seu propósito. 
<a id="configurando_firewall"></a>

### 3.1 Configurando o Firewall

Firewall é um recurso de segurança de rede que protege seu sistema de ataques de invasão. Embora de extrema importância, ele por si só não será o suficiente para evitar todo o tipo de ameaças mas será um grande aliado.

**Nota de Segurança:** 
   
   Esta é uma configuração que pode ser feita ao final, depois de ter sucesso em configurar o servidor e conseguir usá-lo como planejado. Lembrando que assim que for habilitado todos os acessos serão perdidos até que se libere as portas novamente.

No Linux o firewall é configurado pelo **ufw**. O comando `sudo ufw enable` habilita o Firewall e `sudo ufw disable` para desabilitá-lo.

~~~shell
#Ative o ufw
$ sudo ufw enable
[sudo] password for operador: 
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
~~~

Pronto, o firewall já está ativo, agora deve-se configurar as regras para cada serviço. Caso já tenha instalado o *ssh* durante a instalação do *Ubuntu*, já se pode configurar seu acesso criando uma regra conforme abaixo. 

~~~shell
#liberar o firewall para o ssh:
$ sudo ufw allow ssh
Rule added
Rule added (v6)
~~~

Por padrão o *ssh* usa a porta **22** então é possível liberar diretamente indormando a porta. Como exemplo libere a porta **80** que é usada pelo protocolo **HTTP**

~~~shell
operador@siscasa:~$ sudo ufw allow 80 
Rule added
Rule added (v6)
~~~

Também é possível criar regra para negar acessos de um determinado **IP** por exemplo.

~~~shell
operador@siscasa:~$ sudo ufw deny from 192.168.0.10
Rule added
~~~

Para ver as suas regras basta usar o comando `sudo ufw status verbose`:

~~~shell
operador@siscasa:~$ sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----                
22/tcp                     ALLOW IN    Anywhere                  
80                         ALLOW IN    Anywhere  
Anywhere                   DENY IN     192.168.0.10                            
22/tcp (v6)                ALLOW IN    Anywhere (v6)                     
80 (v6)                    ALLOW IN    Anywhere (v6)
~~~

Outra opção é usar o comando `sudo ufw status numbered` para mostrar a lista numerada

~~~shell
operador@siscasa:~$ sudo ufw status numbered
Status: active

     To                         Action      From
     --                         ------      ----                
[ 1] 22/tcp                     ALLOW IN    Anywhere                                                   
[ 2] 80                         ALLOW IN    Anywhere                  
[ 3] Anywhere                   DENY IN     192.168.0.10                          
[ 4] 22/tcp (v6)                ALLOW IN    Anywhere (v6)                          
[5] 80 (v6)                    ALLOW IN    Anywhere (v6) 
~~~

Para deletar regras é possível informar esta numeração conforme abaixo

~~~shell
operador@siscasa:~$ sudo ufw delete 3
Deleting:
 deny from 192.168.0.10
Proceed with operation (y|n)? y
Rule deleted
~~~

Ou pode-se usar diretamente o nome da regra

~~~shell
operador@siscasa:~$ sudo ufw delete allow 80
Rule deleted
Rule deleted (v6)
~~~

Ao final de tantas alterações é importante verificar se tudo está da forma que deseja

~~~shell
operador@siscasa:~$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----                 
22/tcp                     ALLOW       Anywhere                                                
22/tcp (v6)                ALLOW       Anywhere (v6)             
~~~

<a id="configurando_ip_fixo"></a>

### 3.2 Configurando IP estático

O IP é um protocolo de transação de dados que atribui um número único ao computador o identificando tanto na rede local quanto na internet. Em uma rede local geralmente o IP é configurado como Dinâmico, sendo alterado de forma recorrente e aleatória. Caso seu servidor esteja com o IP dinâmico será necessário descobrir qual é o novo IP atribuido a ele toda vez que for alterado. Configurando o IP de forma estática isto não ocorrerá, sempre que precisar acessar o servidor o IP será o mesmo. 

Com o comando `ip a` é possível decobrir o nome da placa de rede usada e que neste exemplo é **enp3s0**

~~~shell
$ ip a
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

No código acima também é possível ver o retorno do comando `ip a` informando o *ip* do servidor como sendo 192.168.0.78

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

Copie o código abaixo e cole dentro do arquivo de configuração que acabou de criar e depois atualize os campos abaixo:

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
Teste a configuração com o comando `netplan try`. Caso tenha dado tudo certo a mensagem abaixo será mostrada. Clique **ENTER** conforme solicitado e aplique a as alterações com o comando `netplan apply`.

~~~shell
root@siscasa:/etc/netplan# netplan try
Do you want to keep these settings?


Press ENTER before the timeout to accept the new configuration


Changes will revert in  98 seconds
Configuration accepted.
root@siscasa:/etc/netplan# netplan apply
~~~

<a id="ssh"></a>

### 3.3 Usando SSH

O **ssh** (Secure Socket Shell) é um protocolo que usa a arquitetura cliente-servidor para criar um canal seguro sobre uma rede insegura. O uso mais comum desta tecnologia é acessar outros computadores de forma remota e será justamente esta função que usaremos neste tutorial. Outro uso para o *ssh* é se autenticar em sistemas. O **GitHub** por exemplo tem esta opção de acesso, permitindo que se possa atualizar um reposítório enviando as atualizações de um repositório local (seu computador) para o repositório remoto do *Github* através de uma chave ssh cadastrada.

O *ssh* geralmente já está presente em vários sistemasatuais, em especial nas distribuições linux, MacOS e outros projetos derivados do [unix](https://pt.wikipedia.org/wiki/Unix) como o **FreeBSD**. A partir do Windows 10 o *ssh* também já está presente e pode ser usado como no linux através do **CMD** ou **PowerShell**. Caso esteja com verções mais antigas uma boa sugestão é usar o [PuTTY](https://www.putty.org/). 

O **Ubuntu** server já dá a opção de instalar o programa SSH que permite acesso seguro ao computador remotamente. Caso não tenha instalado use os comandos:

* `sudo apt install opensh-server` para instalar o programa; 
* `sudo service ssh start` para iniciar o serviço;
* `sudo ufw allow ssh` para liberar o *Firewall*
* `ps aux | grep ssh` para testar o programa.

Como alteramos o IP para **192.168.0.101** basta usá-lo para conectar através do *SSH*. Caso não tenha trocado ainda ou queira confirmar, use o comando `ip a` para isto.

Tendo o IP correto já é possivel acessar o servidor usando o SSH mas é possivel seguir com a configuração diretamente no servidor ou então pode  desligá-lo; colocá-lo ao lado do seu roteador de internet; conectá-los usando um cabo de rede e ligar o PC novamente sem monitor ou teclado.

De um outro computador, abra o terminal se for um Linux e escreva *sudo ssh nome_do_usuário@ip*, caso seja Windows, abra o CMD ou o Powershell e escreva a mesma coisa mas sem usar o *sudo*.

~~~shell
ph@A320:~$ sudo ssh operador@192.168.0.101
The authenticity of host '192.168.0.101 (192.168.0.101)' can't be established.
ECDSA key fingerprint is SHA256:+lySxZ6u2RtnXtoQ9Oo1rHZ5c/feCiaQcuO8iNgLqj8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.0.101' (ECDSA) to the list of known hosts.
operador@192.168.0.101's password: 
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
$ 
~~~

 A linha `$` indica que a partir de agora você está dentro do servidor onde o seu usuário é *operador* e o nome do servidor é *siscasa* que foram definidos na instalação do Ubuntu. Note também que nem sempre, ao digitar uma senha no terminal Linux, aparecerá '*' para cada tecla clicada. 

nota de segurança

<a id="usuario_grupo"></a>

### 3.4 Usuários e grupos

Usuários e grupos são dois conceitos presentes em vários sistemas operacionais. Os usuários são criados para dar acesso ao sistema à pessoas ou serviços e os grupos controlam as permissões para um conjunto de usuários vinculados a ele.

Como já dito, ao instalar o *Linux* é solicitado a criação de um usuário. Este usuário recebe várias permições automaticamente, fazendo dele um usuário bastante podereso. Com ele será possível acessar as permições do usuário **root**, que é o usuário de administração do sistema, através do comando *sudo*.

Na instalação foi criado o usuário **operador** com a senha **123456**. Ele será usado pro todo este tutorial, e por vezes o usuário *root*, para realizar as configurações.

~~~shell
operador@siscasa:~$ sudo -i
[sudo] password for operador: 
root@siscasa:~# 
~~~

Recaptulando, acima está como acessar o usuário *root*. Digite `sudo -i`, Abaixo aparecerá menságem solicitando sua senha. Este campo não mostra `*` para cada caracter digitado. Clique em *enter* em seguida e perceba que o início da linha mudará , neste exemplo foi de **operador@siscasa:~$** para **root@siscasa:~#**, indicando que o usuário mudou de *opedador* para *hoot* e o final mudou para `*`.

Muito cuidado ao usar o usuário *root*. você pode danificar criticalmente seu sistema linux ao fazer alguma alteração indevida. Existem comandos já conhecidos que se executados irão destruir seu sistema como o comando `rm -rfv /` (**NÃO EXECUTE ESTE COMANDO**) que remove todos os arquivos desde a pasta raiz (diretório **/**).

<a id="gerencia_usuario"></a>

#### 3.4.1 Gerenciando usuários

Primeiro passo será criar usuários novos. Serão criados dois novos unsuários:

usuário = leo

senha = 654321

usuário = bia

senha = 112233

Todos os usuários e as respectivas senhas estão aqui pois trata-se de um tutorial didático. Nunca divulgue senhas seja lá qual for e não use senhas tão óbvias, ainda mais para usuários que podem acessar as permições de administrador do sistema.

existem várias formas de se criar usuários, tem como criá-los já atribuindo senha ou criar mais de um usuário por vez. Para facilitar o entendimento será usada uma das formas mais prolixas possível.

Para os dois blocos de códigos abaixo foi usado `useradd` para criar os usuários *leo* e *bia* onde `-s` configura o bash como o shell a ser usado pelo usuário. Em seguida o comando `passwd` permite a configuração da senha do usuário. Para logar com o usuário *bia* por exemplo use o comando `su leo`.

~~~shell
operador@siscasa:~$ sudo useradd leo -s /bin/bash
operador@siscasa:~$ sudo passwd leo
New password: 
Retype new password: 
passwd: password updated successfully
~~~

~~~shell
operador@siscasa:~$ sudo useradd bia -s /bin/bash
operador@siscasa:~$ sudo passwd bia
New password: 
Retype new password: 
passwd: password updated successfully
~~~

Para deletar o usuário *bia* por exemplo digite `userdel bia`.

<a id="gerencia_grupo"></a>

#### 3.4.2 Gerenciando grupos

Com qualquer um destes usuários você não consiguirá atualizar o sistema com `apt` por exemplo pois não estão configurados como usuários **sudo**. Abaixo visualizamos os usuários cadastrados como *sudo*

~~~shell
operador@siscasa:~$ cat /etc/group | grep sudo
sudo:x:27:operador
~~~

O retorno do comando mostra que somente 0 usuário *operador* está cadastrado. Com o comando `adduser` adicionamos o usuário *leo* ao grupo *sudo* e verificamos novamente para certificar que a alteração foi concluída. Desta forma será possivel atualizar o sistema usando o usuário *leo*

~~~shell
operador@siscasa:~$ sudo adduser leo sudo
Adding user `leo' to group `sudo' ...
Adding user leo to group sudo
Done.
operador@siscasa:~$ cat /etc/group | grep sudo
sudo:x:27:operador,leo
~~~

O comando `deluser` abaixo é usado para remover o usuário do grupo *sudo* e depois verificamos se de fato foi removido. caso tente novamente atualizar o sistema será impedido.

~~~shell
operador@siscasa:~$ sudo deluser leo sudo
Removing user `leo' from group `sudo' ...
Done.
operador@siscasa:~$ cat /etc/group | grep sudo
~~~

Agora será criado um grupo com o objetivo de melhor gerenciar os usuários dos serviços instalados futuramente. O nome deste grupo será *user_server*. Com o comando `groupadd` foi criado o grupo e depois verificado se a criação obteve êxito através do comando `cat /etc/group` e filtrando com `| grep user_server` pelo nome do novo grupo. Para excluir um grupo use o `groupdel`. Não é recomendados deletar grupos que foram criados pelo sistema.

~~~shell
operador@siscasa:~$ sudo groupadd user_server
operador@siscasa:~$ cat /etc/group | grep user_server
user_server:x:1003:
operador@siscasa:~$ 
~~~

Uma vez o grupo tenha sido criado, basta adicionar os unuários à ele.

~~~shell
operador@siscasa:~$ sudo adduser operador user_server 
Adding user `operador' to group `user_server' ...
Adding user operador to group user_server
Done.
operador@siscasa:~$ sudo adduser leo user_server 
Adding user `leo' to group `user_server' ...
Adding user leo to group user_server
Done.
operador@siscasa:~$ sudo adduser bia user_server 
Adding user `bia' to group `user_server' ...
Adding user bia to group user_server
Done.
~~~

Ao finalizar esta tarefa, verifique se todos constam no arquivo */etc/group* como pertencentes ao grupo.

~~~shell
operador@siscasa:~$ cat /etc/group | grep user_server
user_server:x:1003:operador,leo,bia
~~~

<a id="configuracoes_especiais"></a>

### 3.5 Configurações opcionais

As configurações a seguir serão necessárias caso tenha mais de uma partição disponível ou esteja usando um **Notebook** para montar este servidor. É necessário que monte de forma automática a partição ao ligar o servidor e configurar a tampa do notebook para que não suspenda ou desligue o servidor quando este estiver com a tampa fechada.

<a id="montando_particao"></a>

#### 3.5.1 Montando partição automaticamente ao ligar o servidor

Caso tenha particionado seu dispositivo de armazenamento ou tenha mais de um dispositivo instalado, seja um **SSD** ou **HD**, será necessário montar esta partição automaticamente ao iniciar o servidor, se não sempre que o servidor for iniciádo deverá montar as partições no local apropriado. É o tipo de configuração que não é necessária quando está usando o *Linux* no seu PC particular a menos que tenha algum programa que use os arquivos salvos na partição em questão e precise que sejam disponibilizados o quanto antes.

Use o comando `lsblk` para listar os dispositivos de armazenamento e suas informações relevantes, incluíndo o **ponto de montagem**.

~~~shell
operador@siscasa:/$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0    7:0    0  63,5M  1 loop /snap/core20/2015
loop1    7:1    0 111,9M  1 loop /snap/lxd/24322
loop2    7:2    0   103M  1 loop /snap/lxd/23541
loop3    7:3    0  49,6M  1 loop /snap/snapd/17883
loop4    7:4    0  40,8M  1 loop /snap/snapd/20092
loop5    7:5    0  63,2M  1 loop /snap/core20/1695
sda      8:0    0 111,8G  0 disk 
├─sda1   8:1    0     1M  0 part 
├─sda2   8:2    0    50G  0 part /
├─sda3   8:3    0    10G  0 part [SWAP]
└─sda4   8:4    0    51G  0 part 
~~~

É possível ver que existe um *dispositivo de armazenamento* denominado **sda** com 111,8 GB. Este disco está dividido em 4 partições nomeadas de *sda1* até *sda4*. O sistema está instalado na partição *sda2* onde podemos ver que está montada na pasta **/**. A partição *sda2* é usada como memória **SWAP** e a *sda4* não está montada. Será esta a partição usada para ser montada automaticamente.

O comando `sudo fdisk -l` mostra informações mais detalhadas sobre os dispositivos instalados. Com ele é possível ver em qual pasta está cada partição dos dispositivos. O retorno deste comando costuma ser bem longo, por isso é recomendado o uso primeiro de `lsblk` para já ter uma nossão do que procurar dentro do *fdisk*. Abaixo está reproduzido seu retorno apenas da parte que diz respeito ao dispositivo de interesse, *sda*.

~~~shell
Disk /dev/sda: 111,79 GiB, 120034123776 bytes, 234441648 sectors
Disk model: SSD 120GB
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: CA96EB09-1097-4B27-B5BF-0B03ECA5CFA6

Device         Start       End   Sectors Size Type
/dev/sda1       2048      4095      2048   1M BIOS boot
/dev/sda2       4096 104861695 104857600  50G Linux filesystem
/dev/sda3  104861696 125833215  20971520  10G Linux swap
/dev/sda4  125833216 232787967 106954752  51G Linux filesystem
~~~

Acima é possível ver que a partição *sda4* está no diretório **/dev/sda4**, esta informação será útil para montar a partição, mas antes deve-se criar uma pasta para ser usada como ponto de montagem.

Use o comando `cd /` para navegar até a pasta raiz do sistma. Com o comando `ls`, liste todas as pastas existentes. 

~~~shell
operador@siscasa:/$ cd /
operador@siscasa:/$ ls
bin   dev  home  lib32  libx32      media  opt   root  sbin  srv       sys  usr
boot  etc  lib   lib64  lost+found  mnt    proc  run   snap  swap.img  tmp  var
~~~

As pastas **media** e **mnt** são destinadas a montagem das partições, sendo *media* a pasta usada quando o sistema monta uma partição (ao iniciar ou quando se conecta um pendrive em distribuições com interface gráfica por exemplo) e a pasta *mnt* é destinada às partições montadas pelo usuário usando o terminal ou dispositivos temporários. Na prática pode-se criar em qualquer local mas como trata-se de um servidor é bom seguir as recomendações e boas práticas. Tendo isto, será criada uma pasta dentro de *media* e nomeada de *sda4* para facilitar a identificação da partição lá montada.

~~~shell
operador@siscasa:/$ cd media
operador@siscasa:/media$ ls
operador@siscasa:/media$ sudo mkdir sda4
operador@siscasa:/media$ ls
sda4
~~~

O comando acima entra na pasta *media* e verifica se já existe alguma pasta salva. Após, cria com `mkdir` a pasta *sda4* e lista novamente para certificar que foi criada.

~~~shell
operador@siscasa:/media$ sudo mount /dev/sda4 /media/sda4
operador@siscasa:/media$ cd sda4
operador@siscasa:/media/sda4$ ls
lost+found
~~~

Já este trecho acima usa as informações dos comandos `lsblk` que foi usado para identificar as partições disponíveis e que não estavam montadas e do comando `fdisk` usado para identificar o diretório da partição, para montar a partição **sda4** na pasta *sda4*. Observe que o caminho da pasta deveser completo deswde o diretório raiz (/media/sda4). Depois entra na pasta e lista os arquivos.

Repare que:

* **/dev/sda4**: Diretório onde está a partição no sistema. Ela não está disponível para uso neste local, necessitando sua montagem em outro diretório;
* **/media/sda4**: Pasta criada para montar a partição *sda4* e torná-la disponível após a montagem.

Caso use o comando `mount`apenas, será possível verificar as partições montadas mas seu retorno costuma ser longo e confuso, a melhor opção é usar novamente o comando *lsblk* para isto. O comando `sudo umount /dev/sda4` pode ser usado para desmontar a partição, observe que não é preciso o caminho da pasta onde a partição foi montada.

~~~shell
operador@siscasa:~$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0    7:0    0   103M  1 loop /snap/lxd/23541
loop1    7:1    0 111,9M  1 loop /snap/lxd/24322
loop2    7:2    0  40,8M  1 loop /snap/snapd/20092
loop3    7:3    0  63,5M  1 loop /snap/core20/2015
loop4    7:4    0  49,6M  1 loop /snap/snapd/17883
loop5    7:5    0  63,2M  1 loop /snap/core20/1695
sda      8:0    0 111,8G  0 disk
├─sda1   8:1    0     1M  0 part
├─sda2   8:2    0    50G  0 part /
├─sda3   8:3    0    10G  0 part [SWAP]
└─sda4   8:4    0    51G  0 part /media/sda4
~~~

Acima o retorno do comando *lsblk* onde é possível notar que a partição *sda4* foi montada na pasta */media/sda4*. Até o momento foram feitos os ajustes para que se possa montar a partição e depois a montagem em sí foi feita de forma manual. Para que seja feita a montagem de forma automática é preciso configurar o arquivo **fstab** que fica na pasta **/etc**. Este arquivo é bem sensível, seu preenchimento errado pode fazer com que o sistema não inicie na próxima vez que for liniciádo então é de suma importância criar uma cópia do arquivo antes de fazer qualquer alteração.

~~~shell

$ cd /etc
$ sudo cp fstab bk_fstab
$ sudo nano fstab
~~~

Os comandos acima são para navegar até a pasta *etc*, criar uma cópia do arquivo e depois usa o comando *sudo nano* para abrí-lo permitindo edição. Deve-se informar os valores para:

`<file system> <mount point>   <type>  <options>       <dump>  <pass>`

Esta linha já está salva no arquivo, está iniciada por `#` o que a transforma em comentário, são os nomes de cada campo que se deve informar.

No final do arquivo foi incluído a linha abaixo com seus valores separados por *tab*

~~~shell
/dev/sda4	/media/sda4	ext4	user,auto,rw,exec	0	0
~~~

Faça as alterações que forem necessárias para adapitá-la ao seu caso de acordo com a explicação seguinte. 

* **file** system (coluna 1): o local do dispositivo que será montado = /dev/sda4;
* **mount** point (coluna 2): Onde a montagem será feita = /media/sda4;
* **type** (coluna 3): O formato usado para formatar a partição = ext4 (outra opção é usar *auto* o que deixa para o sistema identificar qual tipo usar);
* **options** (coluna 4): montado para o grupo de usuário *users* = users, de forma automática ao ligar o pc = auto, permitindo a leitura e escrita = rw, com permissão para rodar executáveis  = exec (este campo é separado por vírgula ficando da seguinte forma: *users,auto,rw,exec*);
* **dump** (coluna 5): Configurado para não fazer Dump = 0;
* **pass** (coluna 6): Configurado para não fazer verificação e reparo = 0.

clique em **Ctrl + s** para salvar e depois **Ctrl + x** para fechar o arquivo. Reinicie o servidor usando o comando `reboot`, quando logar novamente verifique com `lsblk` se a partição foi montada automaticamente.

Caso tenha preenchido algo errado no arquivo *fstab* o sistema provavelmente não irá iniciar, sendo direcionado para o **modo de emergência**. Aparecerá uma tela informando o ocorrido. Digite **Ctrl + D** para abrir o terminal, Vá até a pasta */etc* e analise o arquivo *fstab* em busca do possível erro.

~~~shell
$ sudo rm fstab
$ sudo cp bk_fstab fstab
$ reboot
~~~

Caso não consiga resolver, delete o arquivo e copie o arquivo que deixou de backup mas desta vez para o nome correto conforme coódigo acima. Reinicie novamente, se o sistema abrir é porque realmente o arquivo estava configurado errado. Tente novamente assim que for possível ou inadiável.



<a id="tampa_notebook"></a>

#### 3.5.2 Configurando a tampa do Notebook

Esta configurtação deve ser feita se foi usado um *Notebook* para montar o servidor. Ela evita que o sistema desligue caso a tampa do aparelho seja fechada, configuração importante para manter sempre o Notebook ativo mesmo que com a tampa abaixada, permitindo que fique discretamente ao lado do servidor na sala ou em algum outro canto da casa. Uma vez a tampa abaixada é importante que se desligue ela, econumisando energia.

Para que o *Notebook* não desligue será atualizado o arquivo *logind.conf* que está no diretório */etc/systemd/*. Navegue até a pasta e depois crie um arquivo de *backuop*. Abra o arquivo com `sudo nano` e procure todas as linhas que iniciem com **HandleLidSwitch**, mude seus valores para **ignore**.
~~~shell
operador@siscasa:/media/sda4/samba$ cd /etc/systemd/
operador@siscasa:/etc/systemd$ sudo cp logind.conf bk_logind.conf 
operador@siscasa:/etc/systemd$ ls
bk_logind.conf  network        resolved.conf  system.conf     user.conf
journald.conf   networkd.conf  sleep.conf     timesyncd.conf
logind.conf     pstore.conf    system         user
operador@siscasa:/etc/systemd$ sudo nano logind.conf 
~~~

Após salvar e sair do arquivo use `cat logind.conf | grep HandleLidSwitch` para verificar se todas as linhas foram alteradas corretamente. Estando tudo configurado corretamente, reinicie o serviço com `systemctl restart systemd-logind.service`. 

~~~shell
operador@siscasa:/etc/systemd$ cat logind.conf | grep HandleLidSwitch
HandleLidSwitch=ignore
HandleLidSwitchExternalPower=ignore
HandleLidSwitchDocked=ignore
operador@siscasa:/etc/systemd$ systemctl restart systemd-logind.service
~~~

No arquivo *grub* será configurado o desligamento da tela para economizar energia.  Navegue até a pasta */etc/default/* e lá faça o backup do arquivo e em seguida abra o arquivo para editá-lo, nele mude a linha `GRUB_CMDLINE_LINUX=""` para `GRUB_CMDLINE_LINUX="consoleblank=300"`.

~~~shell
operador@siscasa:~$ cd /etc/default/
operador@siscasa:/etc/default$ sudo cp grub bk_grub
operador@siscasa:/etc/default$ sudo nano grub
~~~

Salve e saia do arquivo, pode usar o comando `cat` para verificar se a alteração foi realizada. Depois reinicie o serviço com `sudo update-grub`.

~~~shell
operador@siscasa:/etc/default$ sudo update-grub
~~~

## 4 Atribuindo funcionalidades

Até este ponto só foram feitas configurações que asseguram o bom funcionamento do servidor ao longo do tempo ou para usar todos os dispositivos de armazenamento de torma automática. Nesta parte é apresentada sugestões de uso para o servidor sendo eles o uso como servidor de arquivos ao compartilhar pasta usando o protocolo **SMB** o que permite também o acesso através de computadores *Windows*, e um banco de dados utilizando o PostgreSQL.

<a id="samba"></a>

### 4.1 Instalando o SAMBA

~~~shell
$ sudo apt install samba
$ cd /etc/samba
operador@siscasa:/etc/samba$ ls
gdbcommands  smb.conf  tls
operador@siscasa:/etc/samba$ sudo cp smb.conf bk_smb.conf 
operador@siscasa:/etc/samba$ ls 
bk_smb.conf  gdbcommands  smb.conf  tls
operador@siscasa:/etc/samba$ sudo nano smb.conf 
$ sudo testparm

~~~

`workgroup = user_server`

~~~shell
workgroup = WORKGROUP
netbios name = siscasa


[arquivos]
   comment = pasta para compartilhamento de arquivos
   path = /media/sda4/samba/arquivos
   public = no 
   writable = yes
   force group = user_server
   create mask = 0770
   directory mask = 0770
   valid users = @user_server

[privados]
   comment = pasta para guardar arquivos de forma privada
   path = /media/sda4/samba/privados     
   read only = no
   create mask = 0700
   directory mask = 0700
   valid users = @user_server
~~~

~~~shell
$ cd /etc/samba/
operador@siscasa:/etc/samba$ ls
bk_smb.conf  gdbcommands  smb.conf  tls
operador@siscasa:/etc/samba$ sudo nano smb.conf 
[sudo] password for operador: 
operador@siscasa:/etc/samba$ sudo service smbd restart 
[sudo] password for operador: 
operador@siscasa:/etc/samba$ sudo ufw allow samba
Rule added
Rule added (v6)
operador@siscasa:/etc/samba$ 
operador@siscasa:/etc/samba$ sudo smbpasswd -a operador
New SMB password:
Retype new SMB password:
Added user operador.
operador@siscasa:/etc/samba$ systemctl status smbd
~~~

~~~shell
operador@siscasa:~$ sudo smbpasswd -a operador
New SMB password:
Retype new SMB password:
operador@siscasa:~$ sudo smbpasswd -a leo
New SMB password:
Retype new SMB password:
operador@siscasa:~$ sudo smbpasswd -a bia
New SMB password:
Retype new SMB password:
Added user bia.
operador@siscasa:~$ sudo smbpasswd -a eva
New SMB password:
Retype new SMB password:
Failed to add entry for user eva.
operador@siscasa:~$ sudo pdbedit -L -v | grep username
Unix username:        operador
NT username:          
Unix username:        bia
NT username:          
Unix username:        leo
NT username:          
~~~


<a id="acesso_samba_windows"></a>

#### 4.1.1 Acessando de uma máquina Windows

abra o Windows Explorer (gerenciador de arquivos do Windows) e na barra de pesquisa escreva:

\\ip\pasta

neste exemplo ficou como:

\\192.168.0.101\sambashare

![Exemplo de uso e configuração samba com Windows #1](https://github.com/silvamathias/servidor_residencial/blob/main/media/samba_win/samba_win_1.png)

Insira o usuário e a senha

clique com o botão direito e selecione a opção *Mapear Unidadew de Rede*

![Exemplo de uso e configuração samba com Windows #2](https://github.com/silvamathias/servidor_residencial/blob/main/media/samba_win/samba_win_2.png)

faça as alterações que julgar necessárias e clique em *OK*

![Exemplo de uso e configuração samba com Windows #3](https://github.com/silvamathias/servidor_residencial/blob/main/media/samba_win/samba_win_3.png)

<a id="acesso_samba_linux"></a>

#### 4.1.2 Acessando de uma máquina Linux

No Gnome Files (Gerenciador de arquivos do Gnome, antigamente chamado de Nautilus) clique Ctrl + l. Já No Thunar (Gerenciador de arquivo do XFCE) já aparece a barra de pesquisa. Clique na barra e escreva:

smb://IP/pasta/

Onde *pasta* é a pasta informada entre **[]** lá no arquivo de configuração do samba (neste caso, 'sambashare' é o nome da pasta)
como o IP cadastrado foi: 192.168.0.101 , então ficaria:

smb://192.168.0.101/sambashare/

![Exemplo de uso e configuração samba com Linux #1](https://github.com/silvamathias/servidor_residencial/blob/main/media/samba_linux/samba_linux_1.png)

depois, coloque o usuário e a senha 

![Exemplo de uso e configuração samba com Linux #2](https://github.com/silvamathias/servidor_residencial/blob/main/media/samba_linux/samba_linux_2.png)

Por fim clique com o botão direito encima da partição do lado esquerdo e selecione Adicionar marcador. Caso não apareça esta opção verifique se a mesma está dentro de  algum outro item ou com outro nome

![Exemplo de uso e configuração samba com Linux #3](https://github.com/silvamathias/servidor_residencial/blob/main/media/samba_linux/samba_linux_3.png)

<a id="instalando_postgre"></a>

### 4.2 Instalando o PostgreeSQL

<a id="Instalando_mais_atual"></a>

#### 4.2.1 Instalando a verção mais recente

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

<a id="habilitando_firewall"></a>

#### 4.2.2 Habilitando a porta  do Firewall

~~~shell
#liberar o firewall para o postgre:
$ sudo ufw allow postgresql
Rule added
Rule added (v6)

#reinicie o serviço postgre:
$ sudo systemctl restart postgresql.service 
~~~

<a id="alterando_senha_postgre"></a>

#### 4.2.3 Alterando a senha do Postgre

Ao instalar o **PostgreSql** também é instalado o CLI **psql** para que se possa acessar o banco usando o terminal.

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

<a id="criando_e_acessando_bd"></a>

#### 4.2.4 Criando e acessando um banco de dados

Só será possível criar um banco de dados usando o psql. Caso não tenha certeza do banco que deseja criar, siga o exemplo abaixo mas saiba que terá que acessar desta forma para criar outros banco.

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

<a id="configurando_acesso_postgre"></a>

#### 4.2.5 Configurando o acesso ao PostgreSql

~~~shell
# Ir até a pasta com os arquivos para configuração. Perceba que a pasta **'16'** deve ser trocada caso sua versão não seja a 16 
$ cd /etc/postgresql/16/main/

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

Para este exemplo foi incluido uma linha de comentário, iniciada com *#*, para explicitar que a configuração abaixo foi criada pelo usuário. O resultado final ficou conforme abaixo:

~~~shel
# configuracao manual
host    all        all        0.0.0.0/0    md5
~~~

Depois de salvar e sair de cada um dos arquivos, reinicie o **PostgreSql** com o comando `sudo systemctl restart postgresql.service` 

<a id="acesso_remoto_postgre"></a>

#### 4.2.6 Acessando o banco remotamente?

Até o momento o **PostgreSql** só foi acessado através do psql pois está logado no servidor por uma conexão remota com ssh ou fisicamente com um acesso direto ao servidor fisicamente. Para que se possa acessar o banco de uma forma mais amigável ao usuário comum e mais segura do ponto de vista do servidor. Para isto é utilizado uma **IDE** (Integrated Development Environment) própriada para a administração e uso do bancos de dados. É recomendado que se use o [DBeaver](https://dbeaver.io/download/); Com ela será possível acessar o banco de dados sem a necessidade de conceder acesso também ao servidor através do *ssh*. 

*DBeaver* é um cliente SQL multiplataforma, com versões para Linux, Windows e Mac, possibilitando o acesso e o gerenciamento de diversos bancos de dados, entre eles o **PostgreSql**. Basta realizar a instalação, abrir o programa e criar uma conexão nova informando **o host, o banco de dados, o usuário e a senha**, conforme abaixo. O campo *URL* será atualizado automaticcamente.

![Exemplo de uso e configuração da conexão usando o *DBeaver*](https://github.com/silvamathias/servidor_residencial/blob/main/media/postgre/conexao_postgre.png)

## 5 Considerações finais

<a id="aprofundando_conhecimento"></a>

### 5.1 Aprofundando conhecimento



~~~shell
sudo apt install cockpit
sudo systemctl start cockpit
sudo systemctl enable cockpit



operador@siscasa:~$ sudo ufw allow 9090
Rule added
Rule added (v6)
operador@siscasa:~$ sudo systemctl restart cockpit

~~~

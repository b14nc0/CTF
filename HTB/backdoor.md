![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/backdoor.jpg)

## Linux - Easy
IP = 10.10.11.125

### USER ###

*** 
Ejecutamos nmap 
    
    nmap -p- --open --min-rate 5000 -vv -n -Pn 10.10.11.125 -oG allPorts
    
![Image text]()

    nmap -sC -sV -p22,80  -oN targeted 10.10.11.125

![Image text]()

Detectamos un wordpress y el servicio ssh
Revisamos las paginas y vemos que resuelve por nombre, añadimos backdoor.htb al /etc/hosts

ejecutamos wfuu para realizar un scaneo de directorios en busca de algo mas de información

    wfuzz -c -t 400 --hc=404 --hh=1785 -L -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://10.10.11.125/FUZZ/
            
![Image text]()

Detectamos varias paginas entre ellas admin accedemos e intentamos conectar con las password por defecto y no funciona.

![]()

Ejecutamos wpscan en busca de vulnerabilidades de wordpress

    
Detectamos un plugin que nos muestra como vulnareble y una pagina en json que nos indica que usa el usuario admin


Vemos que el plugin .... no esta funcionando pero aparece ebookupload
comprobamos en searchsploit y vemos que tiene una vulerabilidad tipo Directory Traversal que nos permite leer ficheros.


Descargamos el wp-config y nos muestra datos de la bbdd


Buscando informacion encontramos la forma de ver los procesos en ejecucion en la maquina, https://www.netspi.com/blog/technical/web-application-penetration-testing/directory-traversal-file-inclusion-proc-file-system/

Procedemos a crear un script para ver que es lo que se ejecuta.




Una vez ejecutado el script encontramos un servicio 



buscamos en searchsploit y descubrimos que es vulnerable a RCE 
ejecutamos el exploit y conseguimos aceceso a la maquina


cambiamos la forma de la shell de la maquina

    script /dev/null -c bash


### ROOT ###

para conseguir acecso a root lo primero que hacemos es sudo -l pero pide password
revisamos los procesos que se estan ejecutando y vemos que hay un while true que esta ejecutando una shell de root con screen.

    screen -x root/root

Y conseguimos acceso a root
leemos la flag.










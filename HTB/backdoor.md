![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/backdoor.jpg)

## Linux - Easy
IP = 10.10.11.125

### USER ###

*** 
Ejecutamos nmap 
    
    nmap -p- --open --min-rate 5000 -vv -n -Pn 10.10.11.125 -oG allPorts
    
![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/allports.jpg)


Detectamos un wordpress y el servicio ssh
Revisamos las paginas y vemos que resuelve por nombre, añadimos backdoor.htb al /etc/hosts

ejecutamos wfuzz para realizar un scaneo de directorios en busca de algo mas de información

    wfuzz -c -t 400 --hc=404 --hh=1785 -L -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://10.10.11.125/FUZZ/
            
![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/wfuzz.jpg)

Detectamos varias paginas entre ellas wp-admin accedemos e intentamos conectar con las password por defecto y no funciona.



Ejecutamos wpscan en busca de vulnerabilidades de wordpress

    wpscan --url http://backdoor.htb -e p,u --plugins-detection aggressive
    
Detectamos un plugin que nos muestra como vulnareble y una pagina en json que nos indica que usa el usuario admin
    
![](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/wpscan2.jpg)

Vemos que el plugin akismet no esta funcionando pero dentro de la caperta de plugins aparece ebookdownlaod
![](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/searchsploit.jpg)

comprobamos en searchsploit y vemos que tiene una vulerabilidad tipo Directory Traversal que nos permite leer ficheros.

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/exploit.jpg)

Descargamos el wp-config y nos muestra datos de la bbdd

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/poc.jpg)

Buscando informacion, encontramos la forma de ver los procesos en ejecucion en la maquina, 
https://www.netspi.com/blog/technical/web-application-penetration-testing/directory-traversal-file-inclusion-proc-file-system/

Procedemos a crear un script para ver que es lo que se ejecuta.

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/script.jpg)

Una vez ejecutado el script encontramos que el servicio gdberver se esta ejecutando en el puerto 1337

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/gdbserver.jpg)

buscamos en searchsploit y descubrimos que es vulnerable a RCE 

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/exploit2.jpg)

![image](https://user-images.githubusercontent.com/42178316/161437073-81a33bf5-058c-47e1-8ee5-ddf7762f92ce.png)


ejecutamos el exploit y conseguimos aceceso a la maquina y la flag de user.txt

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/run.jpg)
![image](https://user-images.githubusercontent.com/42178316/161437091-3e72c002-1e45-4526-ace4-7c7ee10046b9.png)


cambiamos la forma de la shell de la maquina

    script /dev/null -c bash


### ROOT ###

para conseguir acecso a root lo primero que hacemos es sudo -l pero pide password
revisamos los procesos que se estan ejecutando y vemos que hay un while true que esta ejecutando una shell de root con screen.

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/ps.jpg)

    screen -x root/root

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/backdoor/root.txt.jpg)

Y conseguimos acceso a root y acceso a la flag.











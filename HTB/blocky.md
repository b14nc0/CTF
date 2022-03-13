![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/blocky/blocky.jpg)

## Linux - Easy
IP = 10.10.10.37

### USER ###

*** 
Ejecutamos nmap 
    
    nmap  -p- --open --min-rate 5000 -vv -n -Pn -oG allPorts 10.10.10.37
    
![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/blocky/allports.jpg)

Escaneamos los puertos detectados

    nmap  -sC -sV -p21,22,80  -oN target 10.10.10.37

Vemos que tenemos un FTP,SSH y un apache

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/blocky/target.jpg)

realizamos un scaneo de directorios en busca de algo mas de información

   wfuzz -c -t 400 --hc=404,500 -L -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://10.10.10.37/FUZZ
            
![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/blocky/wfuzz.jpg)

Detectamos varias paginas y empezamos a probar.
Intentamos conseguir acceso a phpmyadmin con credenciales por defecto y no funciona
Intentamos conseguir acceso en wp-admin con credenciales por defecto y no funciona
Seguimos investigando el resto de pagina y en plugings detectamos dos ficheros jar, nos los decargamos y los visualizamos

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/blocky/plugins.jpg) 

Para visuarizar un fichero jar lo primero que tenemos que hacer es descomprimirlo
    
    unzip BlockyCore.jar

Una vez que lo hemos descomprimido con javap -c podemos ver el codigo del fichero .class

    javap -c com.myfirstplugin.BlockyCore
    
![](https://github.com/b14nc0/CTF/blob/main/HTB/images/blocky/javap.jpg)

Y vemos que hay un usuario root y una password de sql
Intentamos acceder con root y la password por ssh y no funciona
Intentamos acceder con root y la password en wp-admin y no funciona
Accedemos con las credenciales encontradas en phpmyadmin

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/blocky/phpmyadmin.jpg)

Empezamos a investigar las tablas y vemos que hay una BBDD de wordpress con el usuario noch y una password encriptada

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/blocky/notch.jpg)

probamos a acceder por ssh con notch y la password encontrada anteriormente y conseguimos acceso a la maquina

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/blocky/ssh.jpg)

leemos user.txt

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/blocky/user.jpg)

### ROOT ###

*** 
Ejecutamos sudo -l y vemos que el usuario tiene permisos de root

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/blocky/sudo.jpg)

sudo su

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/blocky/sudosu.jpg)

conseguimos la flag de root.txt

![](https://github.com/b14nc0/CTF/blob/main/HTB/images/blocky/root.jpg)



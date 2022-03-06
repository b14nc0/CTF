## PAPER ##
***

### USER ###

***
Ejecutamos nmap 
    
    nmap 10.10.11.143 -p- --open --min-rate 5000 -v -n -oG allPorts
    
![Image text](https://github.com/b14nc0/images/blob/main/Paper/allPorts.jpg)

    nmap 10.10.11.143 -sC -sV -p22,80,443  -oN targeted

![Image text](https://github.com/b14nc0/images/blob/main/Paper/targeted.jpg)

Detectamos SSH, Apache y SSL estan disponibles
Revisamos la pagina web y vemos que se trata de un NGINX

![Image text](https://github.com/b14nc0/images/blob/main/Paper/nginx.jpg)

    curl -I 10.10.11.143

![Image text](https://github.com/b14nc0/images/blob/main/Paper/curl.jpg)

Detectamos que el X-Backend-Server es office.paper
Lo agregamos al /etc/hosts

![Image text](https://github.com/b14nc0/images/blob/main/Paper/hosts.jpg)

Abrimos en el navegador http://office.paper y vemos que es un blog de una empresa llamada Blunder Tiffin Inc.
Vemos que esta montado en Wordpress

![Image text](https://github.com/b14nc0/images/blob/main/Paper/wappanalizer.jpg)  

Escaneamos la pagina con wpscan

    wpscan --url http://office.paper/ wpscan --url http://office.paper/
    
Detectamos que la version del wordpress 5.2.3 es vulnerable

![Image text](https://github.com/b14nc0/images/blob/main/Paper/wpscan.jpg)

Buscamos en searchsploit vulnerabilidades para la version de wordpress 5.2.3 y encontramos WordPress Core < 5.2.3 - Viewing Unauthenticated/Password/Private Posts 

![Image text](https://github.com/b14nc0/images/blob/main/Paper/searchsploit.jpg)

Explotamos la vulnerabilidad

        http://oficina.papel/?static=1
 
![Image text](https://github.com/b14nc0/images/blob/main/Paper/pagStatic.jpg)

Encontramos una pagina secreta para el registro en un chat

        http://chat.office.paper/register/8qozr226AhkCHZdyY

Editamos el fichero /etc/hosts e incluimos chat.office.papper

![Image text](https://github.com/b14nc0/images/blob/main/Paper/hosts2.jpg)
        
Entramos en la pagina y nos registramos

![Image text](https://github.com/b14nc0/images/blob/main/Paper/pagina%20de%20registro.jpg)

Y nos logamos con el usuario generado

![Image text](https://github.com/b14nc0/images/blob/main/Paper/login.jpg)

Navegamos por la pagina y descubrimos un chat general que en el cual no podemos escribir, pero detectamos que hay un bot recyclops con el que podemos interactuar.

![Image text](https://github.com/b14nc0/images/blob/main/Paper/chatBot.jpg)

Interactuamos con el bot y nos devuelve mensajes para comandos para listar carpetas y abrir ficheros

![Image text](https://github.com/b14nc0/images/blob/main/Paper/bot.jpg)

Listamos caperta con el bot 

![Image text](https://github.com/b14nc0/images/blob/main/Paper/recyclopslist.jpg)

Encontramos una carpeta que se llama hubot 

![Image text](https://github.com/b14nc0/images/blob/main/Paper/recyclopslisthubot.jpg)

Dentro de la carpeta hay un fichero .env y en el nos encontramos una password

![Image text](https://github.com/b14nc0/images/blob/main/Paper/recyclopsFileHubotEnv.jpg)

Probamos a conectarnos por ssh con los datos encontrados en el fichero .env pero no funciona.
Vemos que el dueño de los ficheros listados es dwight probamos con ese usuario y la password encontrada y accedemos.

![Image text](https://github.com/b14nc0/images/blob/main/Paper/ssh.jpg)

Conseguimos la Flag de user.txt

![Image text](https://github.com/b14nc0/images/blob/main/Paper/flagUser.jpg)

### Root ###
***

Probamos sudo su, sudo -l con la password obtenida pero conseguimos nada.
Subimos a la maquina lineas.sh en busca de posibles rutas para escalar privilegios y detectamos que sudo es vulnerable al exploit CVE-2021-3560

![Image text](https://github.com/b14nc0/images/blob/main/Paper/vulnSudo.jpg)

Subimos el exploit CVE-2021-3560
https://github.com/Almorabea/Polkit-exploit/blob/main/CVE-2021-3560.py
y explotamos la vulnerabilidad

![Image text](https://github.com/b14nc0/images/blob/main/Paper/cve20213560.jpg)

Conseguimos acceso con root y leemos la flag

![Image text](https://github.com/b14nc0/images/blob/main/Paper/root.jpg)




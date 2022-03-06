## PAPER ##
### Reconocimiento ###
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

![Image text]()
![Image text]()
![Image text]()
![Image text]()
![Image text]()
![Image text]()

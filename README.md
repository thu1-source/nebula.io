# nebula.io
Tryhackme Nebula.io


**~ Task 2 ~**

nmap -sX -sV -sC -A --reason 10.10.1.111 -p- -oX nebula_vuln.xml --stylesheet="https://svn.nmap.org/nmap/docs/nmap.xsl"
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-09 13:23 EDT
Nmap scan report for 10.10.1.111
Host is up, received echo-reply ttl 63 (0.069s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE REASON       VERSION
53/tcp   open  domain  tcp-response ISC BIND 9.9.5-3ubuntu0.19 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.9.5-3ubuntu0.19-Ubuntu
80/tcp   open  http    tcp-response lighttpd 1.4.33
|_http-title: Bluffer V.0.1a
|_http-server-header: lighttpd/1.4.33
1986/tcp open  ssh     tcp-response OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 77:bd:da:ab:76:ac:f2:e6:5e:89:13:62:d5:64:2c:eb (DSA)
|   2048 a0:ec:8e:db:17:ff:f9:61:ce:68:bb:5d:1c:b4:a8:ba (RSA)
|   256 dd:d8:d6:76:dc:d4:67:7b:15:94:4a:9c:d8:d3:cb:37 (ECDSA)
|_  256 b1:7b:06:a9:49:85:1e:2a:0a:de:71:9d:8b:50:d3:4a (ED25519)
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.4
OS details: Linux 4.4
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 3306/tcp)
HOP RTT       ADDRESS
1   115.51 ms 10.21.0.1
2   117.80 ms 10.10.1.111

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 484.53

**¿Qué tipo se solicitud (flag) manda Nmap para realizar un descubrimiento de host?** SYN

Realizando un nmap -sC -A --reason IPTARGET -p-

Mientras lo monitorizas con Wireshark para poder revisar el trafico de red.

**¿Si descubrimos el host a través de ARP cuál es el primer length que se envía al puerto más pequeño?** 60

Se puede visualizar en Wireshark

**¿De cuanto es el Time-To-Live con que nos responde Nebula?** 63

Se puede visualizar en Wireshark

**~ Task 3 ~**

**¿Qué puerto tiene abierto por UDP?** 53 puerto

**¿Qué tiempo de vida (TTL) tiene el puerto UDP en nuestro host?** 64

Se hace un Nmap -sU -A --reason IPTARGET -p-

**¿Qué puerto tiene corriendo el servicio domain?** 53

**¿Cuántos puertos TCP tiene cerrados?** 65532

En el nmap se visualiza els ports closed

**¿Cuál es el bind.version?** 9.9.5-3ubuntu0.19-Ubuntu

**¿Cuál es el título de la web?** Bluffer V.0.1a

Se tiene que visualizar con http://IPTARGET:80

**¿Por qué puerto corre el servicio licensedaemon?** 1986

En el nmap se visualiza.

**¿Qué versión de SSH tiene Nebula?** OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)

En el nmap se visualiza.

**¿Cuál es la Public Key de ED25519?** b1:7b:06:a9:49:85:1e:2a:0a:de:71:9d:8b:50:d3:4a

**~ Task 4 ~**

**¿Qué servicio es vulnerable a ataques de man-in-the-middle?** SSH

sudo /bin/systemctl start nessusd.service (para inicializar el escaneo de Nessus)

Se realiza un Nessus con un scan basic para poder visualizar las vulnerabilidades.

**¿Cuál es el CVSS asociado a la vulnerabilidad?** 5,9

Se visualiza al acceder a las vulnerabilidades

**¿Cómo se llama popularmente el ataque?** Terrapin Attack

**¿Cuál es el CVE asociado a la vulnerabilidad más alta de SSH?** CVE-2023-48795

**¿Cuál es CVSS más bajo que has encontrado?** 2,1

**¿Cómo se llama la vulnerabilidad?** ICMP Timestamp Request Remote Date Disclosure

https://security.paloaltonetworks.com/CVE-2023-48795 (para ver la vulnerabilidad y que acciones realiza).

**¿Qué Plugin la ha detectado?** #10114

**~ Task 5 ~**

**¿Cuál es el código de verificación de google site?**

nslookup -type=TXT nebula.io 10.10.166.43
Server:         10.10.166.43
Address:        10.10.166.43#53

nebula.io       text = "nebula-verification=examplecode123"
nebula.io       text = "google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA"

__

Para ver el resto de preguntas se hace un dig @iptarget web axfr 
hay que añadir la ip al archivo hosts

~ Task 6 ~

Utilizar GoBuster

Utilizamos el comando 

Gobuster vhost dir -u url -w rutadelwordlist --exclude-lenght 250-400 (para excluir algunos errores notfound, como el 400).

nos saldra status on en verde, si se hace desde kali, iremos a admin.nebula.io (entonces iremos a la carpeta /etc/hosts y dentro pondremos la ip nebula.io y el subdominio
admin.nebula.io

Veremos que pide un PIN, no lo sabemos, entonces nos toca investigar, le damos a inspeccionar (la herramienta de desarrollo) y veremos que sale un .zip en llamado /git_admin.zip

dentro lo descargamos en la carpeta tmp y lo descomprimimos ahi, veremos que hay un .js llamado script.js, si sabemos leerlo pone una variable llamada function que veremos que más abajo cifra el pin con un hash, más arriba del codgo parece que ese hash esta cifrado en 256, tendremos que descifrarlo. Copiamos el hash y lo metemos en esta pagina web francesa: https://www.dcode.fr/funcion-hash-sha256 - Esto lo que hace es descifrar el codigo hash.

Ahora que tenemos el pin accedemos a la pagina. Se nos abre un panel de SIEM para ver las alertas de intrusiones.

Hacemos el SIEM y entonces nos dara unas cuantas palabras, probamos cual es la contraseña por ssh y utilizamos el usuario SSH, hemos probado bluffer y entramos por el puerto 1986 que es el que nos ha dado acceso a SSH por nmap. Entramos, hacemos el juego.

~ Task 7 ~

Nos encontramos que se encuentra con rbash, esto quiere decir que es una shell restrictiva, que no te deja hacer nada de nada.

Pero empezamos hacer pruebas. Vemos que no nos deja hacer cd pero si que nos deja hacer el comando pwd. Hacemos un compgen -c 
que te da un listado de los comandos que puedes hacer. 

Entonces me dije; Existe una vulnerabilidad? vamos a probar variables de entorno, y hemos probado $PATH y $SHELL y parece que te da un pequeño resultado.
Decido hacer un ssh ip@puerto -p 1986 -t bash para probar si podemos evadir de la restricción de bash.

Entonces probamos de hacer un export PATH=/bin:/usr/bin y export SHELL=/bin/sh

ejecutamos la variable $SHELL

y nos sale una linea de comandos como así: $ probamos los comandos permitidos por compgen -c y nos da resultado con éxito!

bluffer@Nebula-server:~$ ls
Restricted Permission
bluffer@Nebula-server:~$ whoami
Restricted Permission
bluffer@Nebula-server:~$ $PATH
bash: /home/bluffer/cmds: Is a directory
Restricted Permission
bluffer@Nebula-server:~$ sh
Command 'sh' is available in '/bin/sh'
The command could not be located because '/bin' is not included in the PATH environment variable.
sh: command not found
Restricted Permission
bluffer@Nebula-server:~$ export PATH=/bin:/usr/bin
bluffer@Nebula-server:~$ export SHELL=/bin/sh
bluffer@Nebula-server:~$ $SHELL
$ ls
cmds
$ whoami
bluffer
$ ls -l
total 4
drwxr-xr-x 2 root root 4096 Apr  9 15:17 cmds
$ 

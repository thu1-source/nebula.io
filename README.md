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

Luego hay otra manera que es la correcta que es con el comando: dig @iptarget web axfr 
hay que añadir la ip al archivo hosts

dig @10.10.129.142 nebula.io axfr    

; <<>> DiG 9.20.7-1-Debian <<>> @10.10.129.142 nebula.io axfr
; (1 server found)
;; global options: +cmd
nebula.io.              7200    IN      SOA     ns1.nebula.io. admin.nebula.io. 2023102501 604800 86400 2419200 604800
nebula.io.              300     IN      HINFO   "Nebula Server" "Linux"
nebula.io.              301     IN      TXT     "nebula-verification=examplecode123"
nebula.io.              301     IN      TXT     "google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA"
nebula.io.              7200    IN      MX      0 mail.nebula.io.
nebula.io.              7200    IN      MX      0 ASPMX.L.GOOGLE.COM.
nebula.io.              7200    IN      MX      10 ALT1.ASPMX.L.GOOGLE.COM.
nebula.io.              7200    IN      MX      10 ALT2.ASPMX.L.GOOGLE.COM.
nebula.io.              7200    IN      MX      20 ASPMX2.GOOGLEMAIL.COM.
nebula.io.              7200    IN      MX      20 ASPMX3.GOOGLEMAIL.COM.
nebula.io.              7200    IN      MX      20 ASPMX4.GOOGLEMAIL.COM.
nebula.io.              7200    IN      MX      20 ASPMX5.GOOGLEMAIL.COM.
nebula.io.              86400   IN      NS      ns1.nebula.io.
nebula.io.              86400   IN      NS      ns2.nebula.io.
nebula.io.              7200    IN      A       192.168.150.144
_sip._tcp.nebula.io.    14000   IN      SRV     0 5 5060 sip.nebula.io.
144.150.168.192.IN-ADDR.ARPA.nebula.io. 7200 IN PTR www.nebula.io.
bluffer.nebula.io.      7200    IN      TXT     "BLUFFER{S3cr3t_DNS_Tr4nsfer_Flag}"
contact.nebula.io.      2592000 IN      TXT     "Para soporte, contactar a admin@nebula.io o llamar al +1 123 4567890"
deadbeef.nebula.io.     7201    IN      AAAA    dead:beef::1
ftp.nebula.io.          7200    IN      A       192.168.150.180
mail.nebula.io.         7200    IN      A       192.168.150.146
ns1.nebula.io.          86400   IN      A       192.168.150.144
ns2.nebula.io.          86400   IN      A       192.168.150.145
office.nebula.io.       7200    IN      A       192.0.2.10
sip.nebula.io.          7200    IN      A       192.168.150.147
vpn.nebula.io.          7200    IN      A       198.51.100.10
www.nebula.io.          7200    IN      A       192.168.150.144
xss.nebula.io.          300     IN      TXT     "user : bluffer"
nebula.io.              7200    IN      SOA     ns1.nebula.io. admin.nebula.io. 2023102501 604800 86400 2419200 604800
;; Query time: 48 msec
;; SERVER: 10.10.129.142#53(10.10.129.142) (TCP)
;; WHEN: Thu Apr 17 02:09:25 EDT 2025
;; XFR size: 30 records (messages 1, bytes 961)


~ Task 6 ~

Utilizar GoBuster

Utilizamos el comando: 

gobuster dir -u nebula.io -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt --exclude-length 250-400
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://nebula.io
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] Exclude Length:          275,297,300,308,315,337,388,353,362,394,274,299,321,365,325,288,306,309,333,385,277,254,322,358,387,298,324,339,363,377,379,389,291,340,357,374,396,397,251,260,280,283,296,349,268,290,335,370,384,284,304,313,316,318,343,398,399,253,273,302,326,342,364,366,382,258,323,354,293,303,341,356,359,360,289,348,259,276,346,367,327,294,391,262,329,330,369,375,400,270,285,336,376,383,390,305,351,352,395,263,320,331,373,393,272,292,257,278,311,256,265,266,287,307,328,334,347,269,279,344,345,372,378,380,381,252,267,310,317,319,392,250,261,301,350,368,255,271,286,314,338,361,282,295,332,355,371,386,281,312,264
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/assets               (Status: 301) [Size: 0] [--> http://nebula.io/assets/]
Progress: 87664 / 87665 (100.00%)
===============================================================
Finished
===============================================================

Vemos que hay una carpeta llamada assets pero dentro de esa ruta por navegador sale error 404. Probamos a ver si hay virtual hosts con la opción vhost:

gobuster vhost dir -u http://nebula.io -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt --append-domain --exclude-length 250-320
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:              http://nebula.io
[+] Method:           GET
[+] Threads:          10
[+] Wordlist:         /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
[+] User Agent:       gobuster/3.6
[+] Timeout:          10s
[+] Append Domain:    true
[+] Exclude Length:   286,303,259,273,319,255,252,256,263,272,282,284,306,269,274,280,281,290,305,320,251,261,267,292,294,260,265,291,296,313,315,250,275,288,298,300,304,308,314,270,276,254,279,293,302,309,317,253,257,277,278,289,307,316,266,295,310,312,262,268,287,258,264,285,299,301,311,318,271,297,283
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
Found: contact_us.nebula.io Status: 400 [Size: 349]
Found: admin.nebula.io Status: 200 [Size: 2369]
Found: FireFox_Reco.nebula.io Status: 400 [Size: 349]
Found: EWbutton_Community.nebula.io Status: 400 [Size: 349]
Found: EWbutton_GuestBook.nebula.io Status: 400 [Size: 349]
Found: strona_6.nebula.io Status: 400 [Size: 349]
Found: strona_11.nebula.io Status: 400 [Size: 349]
Found: strona_1.nebula.io Status: 400 [Size: 349]
Found: strona_14.nebula.io Status: 400 [Size: 349]

nos saldra status on en verde, si se hace desde kali, iremos a admin.nebula.io (entonces iremos a la carpeta /etc/hosts y dentro pondremos la ip nebula.io y el subdominio
admin.nebula.io

Veremos que pide un PIN, no lo sabemos, entonces nos toca investigar, le damos a inspeccionar (la herramienta de desarrollo) y veremos que sale un .zip en llamado /git_admin.zip

dentro lo descargamos en la carpeta tmp y lo descomprimimos ahi, veremos que hay un .js llamado script.js, si sabemos leerlo pone una variable llamada function que veremos que más abajo cifra el pin con un hash, más arriba del codigo parece que ese hash esta cifrado en 256, tendremos que descifrarlo. Copiamos el hash y lo metemos en esta pagina web francesa: https://www.dcode.fr/funcion-hash-sha256 - Esto lo que hace es descifrar el codigo hash.

Ahora que tenemos el pin accedemos a la pagina. Se nos abre un panel de SIEM para ver las alertas de intrusiones.

Hacemos el SIEM y entonces nos dara unas cuantas palabras, probamos cual es la contraseña por ssh y utilizamos el usuario SSH, hemos probado bluffer (por deducción personal al salir en la pagina web, aunque en el comando dig sale), otra manera de hacerlo es con la herramienta Hydra, en ese momento lo desconocía y entonces lo hice manualmente probando contraseña por contraseña y entramos por el puerto 1986 que es el que nos ha dado acceso a SSH por nmap. Entramos, hacemos el juego.

~ Task 7 ~

Nos encontramos que se encuentra con rbash, esto quiere decir que es una shell restrictiva, que no te deja hacer nada de nada.

Pero empezamos hacer pruebas, prueba & error. Vemos que no nos deja hacer cd pero si que nos deja hacer el comando pwd, probamos a poner un ` (acento) y nos lleva a un salto de linea que empieza >. Aprovechamos para realizar un compgen -c que te da un listado de los comandos que puedes hacer. 

Entonces me dije; ¿Existe una vulnerabilidad? vamos a probar variables de entorno, y hemos probado $PATH y $SHELL y parece que te da un pequeño resultado.
Decido hacer un: 

ssh ip@puerto -p 1986 -t bash para probar si podemos evadir de la restricción de bash.

Entonces probamos de hacer un export PATH=/bin:/usr/bin y export SHELL=/bin/sh

ejecutamos la variable $SHELL

y nos sale una linea de comandos como así: $ probamos los comandos permitidos por *compgen -c* y nos da resultado con éxito!

Esta actuación se le llama Escape Shell (Escapa del bash de restricción llamado RBASH) pero no te hace elevación de privilegios vertical.

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

De aqui saltamos a la Task 10, viendo un poco el contenido del sistema con comandos basicos como cat, ls, cd.

Vemos que hay otro usuario llamado guakamole y hay un .txt llamado: warning.txt revisamos y pone: Cuidado con "ryuk"

__

El juego de Bluffer cuando esta cargando le das varias veces al boton del espacio espaciadora y te sale la siguiente información:

***************************************
*                                     *
*         NEBULA.IO PRESENTA          *
*              BLUFFER                *
*                                     *
***************************************

   Un viaje a través de las mazmorras   
       Cargando, por favor espera       

..

Carga interrumpida. Fallo en el sistema
-e 
###
-e 

RYUK V0.02a2 

HANDLER RANSOMWARE FILE
...
EXEC CODING

[*] Connecting to server 10.10.6PmP.@*x4
[*] Connected ...
[*] Authenticating user : fyc5QNQ0twf*mjc2ebr
[*] Authentication successful.
[*] Accessing server resources : kvd2MAV@vxk4mcg!ecv
[*] Downloading sensitive data : pdAFihaBYt6@*x4-T6Qqvq8ph.6PmP
[*] GET /admin/config/settings HTTP/1.1
[*] Host: 127.0.0.1
[*] Hostname: Nebula Server Kernel
[*] Authorization: Bearer : <token> CJcKuhwvsYKx3g9-yM.LwGfJEqXT.2u8co_Cid!.bW8ii8np7_KEgDFegEh34F-F42a6QTEmbPyTg </token>
[*] Data received from server :
---------------------------
root::0:0:root:/root:/bin/bash
admin:x:1:1:admin:/admin:/bin/sh
---------------------------
[*] Injecting malicious code
[*] Sending payload ...
[*] POST /admin/upload HTTP/1.1
[*] Content-Type: application/x-www-form-urlencoded
[*] Payload:

[*] Payload successfully deployed
[*] Encrypted Server ...
-e 

¿Qué es RYUK? Ransomware sofisticado para grandes organizaciones, alto rescate, cifrado crítico, propagación por malware.

~ Task 8 ~

Qué hemos probado en esta TASK?
nmap para ver que puerto ha abierto, la herramienta enum4linux y smbclient.

enum4linux -a 10.10.129.142 
Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Thu Apr 17 04:15:24 2025

 =========================================( Target Information )=========================================

Target ........... 10.10.129.142
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ===========================( Enumerating Workgroup/Domain on 10.10.129.142 )===========================


[+] Got domain/workgroup name: NEBULA_ROCKS


 ===============================( Nbtstat Information for 10.10.129.142 )===============================
                                                                                                               
Looking up status of 10.10.129.142                                                                             
        NEBULA-SERVER   <00> -         B <ACTIVE>  Workstation Service
        NEBULA-SERVER   <03> -         B <ACTIVE>  Messenger Service
        NEBULA-SERVER   <20> -         B <ACTIVE>  File Server Service
        ..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser
        NEBULA_ROCKS    <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
        NEBULA_ROCKS    <1b> -         B <ACTIVE>  Domain Master Browser
        NEBULA_ROCKS    <1d> -         B <ACTIVE>  Master Browser
        NEBULA_ROCKS    <1e> - <GROUP> B <ACTIVE>  Browser Service Elections

        MAC Address = 00-00-00-00-00-00

 ===================================( Session Check on 10.10.129.142 )===================================
                                                                                                               
                                                                                                               
[E] Server doesn't allow session using username '', password ''.  Aborting remainder of tests.    

smbclient -L //10.10.129.142 -p 44544 -N

        Sharename       Type      Comment
        ---------       ----      -------
        nebula_share    Disk      
        IPC$            IPC       IPC Service (Nebula.io File Tansfer Server)
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.129.142 failed (Error NT_STATUS_CONNECTION_REFUSED)
Unable to connect with SMB1 -- no workgroup available

~ Task 9 ~
Usar la deducción. En el juego estaba el comando OPEN_SMB que abre el puerto de smb que usan, y en el 8 se trataba del protocolo smb, a parte cuando realizas otro nessus te sale la vulnerabilidad. Investigue por internet y fui buscando las respuestas, en este ejercicio fui más de analista e investigador.

~ Task 10 ~
Mencionado en la TASK 7, como respondi algunas preguntas. Aunque en esta TASK hay que elevar privilegios y hacer uso de Metasploit.

~ Task 11 ~

Yo sé que se hace con John the Ripper, ya que conozco el programa pero nunca lo he utilizado, sé que descifra hash.

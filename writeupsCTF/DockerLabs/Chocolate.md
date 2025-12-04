# Chocolate

Plataforma: Dockerlabs OS: Linux Level: Easy Status: Done Complete: Yes EJPT: yes Created time: 5 de diciembre de 2024 21:03

Reconeixement

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv  -n 172.17.0.2 -oG allports
```

![image.png](../../.gitbook/assets/image.png)

Mirem codi font i apareix;

![image.png](<../../.gitbook/assets/image 1.png>)

Entrem a :

[http://172.17.0.2/nibbleblog/](http://172.17.0.2/nibbleblog/)

![image.png](<../../.gitbook/assets/image 2.png>)

Trobem panell d'autenticació:

![image.png](<../../.gitbook/assets/image 3.png>)

Credencials per defecte : admin/admin Trobem versió :

![image.png](<../../.gitbook/assets/image 4.png>)

Busquem exploit Provem a metasploit

![image.png](<../../.gitbook/assets/image 5.png>)

Configurem parametres:

Usuari: admin Pass: admin RHOST: 172.17.0.2

Falla Veiem que al scrip utilitza la ruta del Plugin "My\_image" Instale el plugin a la web executem exploit

Entrem i tractem tty:

![image.png](<../../.gitbook/assets/image 6.png>)

Som usuari www-data

```bash
sudo -l :
```

![image.png](<../../.gitbook/assets/image 7.png>)

L'usuari "chocolate" pot utilitzar php

Busquem a GTOBins i torbem:

![image.png](<../../.gitbook/assets/image 8.png>)

Per poder fer-ho amb l'usuari chocolate i que no demani password:

![image.png](<../../.gitbook/assets/image 9.png>)

Som usuari chocolate

Veiem amb ps -faux que corre un script php com a root

![image.png](<../../.gitbook/assets/image 10.png>)

/opt/script.php

Modifiquem el fitxer :

echo '' > /opt/script.php

Comprovem que ha canviat la bash :

![image.png](<../../.gitbook/assets/image 11.png>)

amb el permis sudoer

![image.png](<../../.gitbook/assets/image 12.png>)

Ja amb la bash modificada fem

bash -p root

![image.png](<../../.gitbook/assets/image 13.png>)

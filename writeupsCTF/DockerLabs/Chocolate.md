# Chocolate

Plataforma: Dockerlabs
OS: Linux
Level: Easy
Status: Done
Complete: Yes
EJPT: yes
Created time: 5 de diciembre de 2024 21:03

Reconeixement

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv  -n 172.17.0.2 -oG allports
```

![image.png](/images/DockerLabs/image.png)

Mirem codi font i apareix;

![image.png](/images/DockerLabs/image%201.png)

Entrem a :

[http://172.17.0.2/nibbleblog/](http://172.17.0.2/nibbleblog/)

![image.png](/images/DockerLabs/image%202.png)

Trobem panell d'autenticació:

![image.png](/images/DockerLabs/image%203.png)

Credencials per defecte : admin/admin
Trobem versió :

![image.png](/images/DockerLabs/image%204.png)

Busquem exploit
Provem a metasploit

![image.png](/images/DockerLabs/image%205.png)

Configurem parametres:

Usuari: admin
Pass: admin
RHOST: 172.17.0.2

Falla
Veiem que al scrip utilitza la ruta del Plugin "My_image"
Instale el plugin a la web
executem exploit

Entrem i tractem tty:

![image.png](/images/DockerLabs/image%206.png)

Som usuari www-data

```bash
sudo -l :
```

![image.png](/images/DockerLabs/image%207.png)

L'usuari "chocolate" pot utilitzar php

Busquem a GTOBins i torbem:

![image.png](/images/DockerLabs/image%208.png)

Per poder fer-ho amb l'usuari chocolate i que no demani password:

![image.png](/images/DockerLabs/image%209.png)

Som usuari chocolate

Veiem amb ps -faux que corre un script php com a root

![image.png](/images/DockerLabs/image%2010.png)

/opt/script.php

Modifiquem el fitxer :

echo '<?php exec("chmod u+s /bin/bash"); ?>' > /opt/script.php

Comprovem que ha canviat la bash :

![image.png](/images/DockerLabs/image%2011.png)

amb el permis sudoer

![image.png](/images/DockerLabs/image%2012.png)

Ja amb la bash modificada fem

bash -p
root

![image.png](/images/DockerLabs/image%2013.png)
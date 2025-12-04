# ⚡ Bandit

Level 0-1 :

```bash
cat readme
```

> Flag : ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

Level 1-2:

```bash
cat ./-
cat /home/bandit1/-
```

> Flag : 263JGJPfgU6LtdEvgfWU1XP5yac29mFx

Level 2-3:

```bash
cat "spaces in this filename"
cat *
```

> Flag: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

Level 3-4:

```bash
ls -la
cat ...Hiding-From-You
```

> Flag : 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

Level 4-5:

```bash
1- 
find /home/bandit4/inhere/-file00,-file01,etc..

2-
find /home/bandit4/inhere/ -type f |xargs file | grep text
/home/bandit4/inhere/-file07: ASCII text
'xargs file` will run the `file` command on each of the lines from the piped input.'

cat /home/bandit4/inhere/-file07
```

> Flag: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

Level 5-6:

Find:

* human-readable
* 1033 bytes in size
* not executable

```bash
find /home/bandit5/inhere/ -type f -size 1033c ! -executable
'-type f : file
-size : size in bits
! -executable : exclou que siguin executables'
```

> Flag: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

Level 6-7:

Find:

* owned by user bandit7
* owned by group bandit6
* 33 bytes in size

```bash
find / -type f -user bandit7 -group bandit6 -size 33c |xargs file |grep text
```

> Flag: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

Level 7-8:

The password for the next level is stored in the file **data.txt** next to the word **millionth**

```bash
cat data.txt | grep millionth
```

> Flag: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

Leve 8-9:

```bash

sort data.txt | uniq -u

'sort: ordena alfabeticament el fitxer
 unic : reporta las linies repetides (nomes funciona si les lines duplicades estan consecutives)
 -u: nomes reporta les que surten una vegada'
```

> Flag: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

Level 9-10:

The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

```bash
strings data.txt | grep =====

```

> Flag : FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

Level 10-11:

The password for the next level is stored in the file **data.txt**, which contains base64 encoded data

```bash
base64 -d data.txt
'base64 : tradueix a base64
-d : decode'
```

> Flag: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

Level 11-12:

La contraseña para el siguiente nivel se almacena en el archivo data.txt, donde todas las letras minúsculas (a-z) y mayúsculas (A-Z) se han rotado 13 posiciones.

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
'tr 'A-Za-z' 'N-ZA-Mn-za-m': rota las letras del alfabeto 13 posiciones hacia atrás, decodificando el texto con ROT13.'
```

> Flag: The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4

Level 12-13:

```bash
cd /tmp
mktemp -d
cd /tmp/tmp.W5t1vua6G9
cp ~/data.txt .
mv data.txt hexdump_data
cat hexdump_data | head
xxd -r hexdump_data compressed_data
cata compressed_data | head
mv compress_data compressed_data.gz
gzip -d compressed_data.gz
xxd compressed_data
mv compressed_data compressed_data.bz2
bzip2 -d compressed_data.bz2
mv compressed_Data compressed_data.gz
gzip -d compressed_data.gz
mv comrpessed_data compressed_data.tar
tar -xf comrpessed_data.tar
tar -xf data5.bin
xxd data6.bin
bzip2 -d data6.bin
tar -xf data6.bin.out
xxd data8.bin
mv data8.bin data8.gz
gzip -d data8.gz
cata data9
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

> FLag: The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

Level 13-14:

The password for the next level is stored in **/etc/bandit\_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. **Note:** **localhost** is a hostname that refers to the machine you are working on:

```shell
ls
sshkey.private

'Copiamos la sshkey.private a nuestra maquina como : '

ssh_private_key
chmod 600 ssh_private_key

ssh -i ssh_private_key bandit14@bandit.labs.overthewire.org -p 2220

cd /etc/bandit_pass/
cat bandit14

MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

> Flag: MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

Leve 14-15:

The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.

```shell
'Conectamos con el usuario bandit14'

telnet localhost 30000
pass: MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS 'de bandit14'

Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

> Flag: 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

Level 15-16:

The password for the next level can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL/TLS encryption.

**Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.**

```shell
'Conectamos por openssl al localhost por el puerto 30001'

bandit15> openssl s_client -connect localhost:30001
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo ' bandit15 password'

Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

> Flag: kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

Level 16-17:

The credentials for the next level can be retrieved by submitting the password of the current level to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

```shell

'Primero buscamos con nmap que puertos estan abiertos en el rango 31000-32000'

bandot16> nmap -p31000-32000 -sS -sCV -p -Pn localhost

Discovered open port 31960/tcp on 127.0.0.1
Discovered open port 31790/tcp on 127.0.0.1
Discovered open port 31518/tcp on 127.0.0.1
Discovered open port 31691/tcp on 127.0.0.1
Discovered open port 31046/tcp on 127.0.0.1

Develven data: 31518 ,31790
31518 : TLSv1.3: 
31790 : no tls1_3

'Probamos los dos puertos,el que recibe datos es el 31790'

bandit16@bandit:~$ openssl s_client -connect localhost:31790 -quiet -no_tls1_3

Correct!
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
```

Level 17-18:

There are 2 files in the homedirectory: **passwords.old and passwords.new**. The password for the next level is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new**

**NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19**

```shell
'Para comparar dos archivos usamos'

diff archivo1 archivo2

diff passwords.old passwords.new

Output:

< ktfgBvpMzWKR5ENj26IbLGSblgUG9CzB
---
> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

'<: indica que la linea esta presente en el primer archivo pero no en el segundo'
'>: indica que la linea esta presente en el segundo archivo pero no en el primero'

Por lo tanto: x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
```

> Flag: x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

Level 18-19:

The password for the next level is stored in a file **readme** in the homedirectory. Unfortunately, someone has modified **.bashrc** to log you out when you log in with SSH.

```shell
'Vemos que no podemos conectar.
Conectamos con bandit17 y vemos que en bandit18 hay un script que bloquea el acceso:'

# ~/.bash_logout: executed by bash(1) when login shell exits.

# when leaving the console clear the screen to increase privacy

if [ "$SHLVL" = 1 ]; then
    [ -x /usr/bin/clear_console ] && /usr/bin/clear_console -q
fi

'**`$sHLVL`**: Esta es una variable de entorno que puede ser definida en el sistema o en el entorno de ejecución del script. El script está comprobando si su valor es igual a `1`.'

Probamos a cambiar el tipo de shell:

sudo ssh bandit18@bandit.labs.overthewire.org -p 2220 "/bin/sh"
cat readme
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

Alternativa:

sudo ssh -T bandit18@bandit.labs.overthewire.org -p 2220

'-T: evita asignar una psedo terminal'

```

> Flag: cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

Level 19-20:

To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit\_pass), after you have used the setuid binary.

```shell
'Vemos que en el home hay un fichero bandit20-do,con el bit suid , que ejecuta cualquier comando como bandit20'

./bandit20-do id
uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20) groups=11019(bandit19)

./bandit20-do whoami
bandit20

'Por lo tanto, leemos el fichero de /etc/bandit_pass como bandit20 ejecutando:'

./bandit20-do cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

> Flag: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

Level 20 -21:

There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

```shell
 nc -lvp 8080 < /etc/bandit_pass/bandit20
 cntrl+z # Interumpe el proceso sin terminarlo
 bg # Pone el proceso en segundo plano
 ./suconnect 8080

Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
```

> Flag: EeoULMCra2q0dSkYj561DX7s1CpBuOBt

Level 21-22:

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

```shell
cat /etc/cron.d
cat cronjob_bandit22

Output:
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null

cat /usr/bin/cronjob_bandit22.sh

output: 
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
Output: 

tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

> Flag: tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q

Level 22-23:

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

```shell
cat /etc/cron.d
cat cronjob_bandit23.sh
Output:
@reboot bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
cat /usr/bin/cronjob_bandit23.sh
Output:
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

/usr/bin/cronjob_bandit23.sh 
Output:

Copying passwordfile /etc/bandit_pass/bandit22 to /tmp/8169b67bd894ddbb4412f91573b38db3

# Esta guarando el password en el directorio temporal que genera mytarget. Para ver el contenido del usuario bandit23, cambiamos la cadena y el hash, codificando con bandit23

echo I am user bandit23 | md5sum | cut -d ' ' -f 1
Output:
8ca319486bfbbc3663ea0fbe81326349


cat /tmp/8ca319486bfbbc3663ea0fbe81326349
Output:0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
```

> Flag: 0Zf11ioIjMVN551jX3CmStKLYqjk54Ga

> Level 23-24:

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

```shell
cat /etc/cron.d
cat cronjob_bandit24.sh
Output:

#!/bin/bash
myname=$(whoami)
cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*; 
do
	if [ "$i" != "." -a "$i" != ".." ];
	then 
		echo "Handling $i"
		 owner="$(stat --format "%U" ./$i)" 
		 if [ "${owner}" = "bandit23" ]; then 
			 timeout -s 9 60 ./$i 
		 fi rm -f ./$i 
	fi 
done

#- 
 myname=$(whoami):    
	# -Asigna a la variable `myname` el resultado del comando #`whoami`, que devuelve el nombre del usuario que ejecuta el #script.
cd /var/spool/$myname/foo:
    
	# -Cambia el directorio de trabajo actual a #`/var/spool/$myname/foo`, donde `$myname` es el nombre del #usuario obtenido en el paso anterior. Si este directorio no ##existe o no es accesible, el script fallará en este punto.
echo "Executing and deleting all scripts in /var/spool/$myname/foo:":
#    
#    - Muestra un mensaje en la terminal indicando que se están #ejecutando y eliminando scripts dentro del directorio #`/var/spool/$myname/foo`.
for i in * .*;
#    
#    - Este bucle `for` comienza a iterar sobre todos los #archivos y directorios dentro de `/var/spool/$myname/foo`. Usa #`* .*` para incluir tanto archivos normales como los ocultos 
#(que comienzan con un punto).
if [ "$i" != "." -a "$i" != ".." ];
#    
#    - Este `if` verifica si el archivo/dirección iterado no es #`.` (directorio actual) o `..` (directorio anterior). Esto es #para evitar que el script intente ejecutar o eliminar estos #directorios especiales.
echo "Handling $i"
#    
#    - Imprime un mensaje indicando el archivo que se está manejando actualmente.
owner="$(stat --format "%U" ./$i)"
#    
#    - Usa el comando `stat` para obtener el propietario (`%U`) #del archivo actual (`$i`) y lo guarda en la variable `owner`.
if [ "${owner}" = "bandit23" ]; then
#    
#    - Comprueba si el propietario del archivo es el usuario `bandit23`. Si es así, entonces se procederá a ejecutar el archivo.
timeout -s 9 60 ./$i
#    
#    - Si el archivo es propiedad de `bandit23`, lo ejecuta con un límite de tiempo de 60 segundos usando el comando `timeout`. La opción `-s 9` envía la señal `SIGKILL` (9) si el proceso tarda más de 60 segundos en completarse.

rm -f ./$i
    
#    - Finalmente, independientemente de si el archivo fue ejecutado o no, se elimina usando `rm -f`, que fuerza la eliminación sin pedir confirmación.

'Nos quedamos con la parte del script que nos indica que ejecuta los archivos que esten en el directorio actual o en el anterior'
if [ "$i" != "." -a "$i" != ".." ];

'Vemos que tenemos permiso de escritura en el directorio '
find / -type d -perm -o+w 2>/dev/null

/tmp
/var/spool/bandit24/foo
/run/lock
/run/lock/test
/run/screen

'Como es el directorio donde la tarea cron ejecuta y lee los scripts, creamos un script que nos permita leer como el usuario bandit24 el fichero de /etc/bandit_pass/bandit24 y lo guarde en otro directorio que tenga permisos para escribir'

touch prova.sh
#!bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/bandit24.txt
#
chmod +x prova.sh
cd /usr/bin/
./cronjob_bandit24.sh
cd /tmp
cat bandit24.txt
Output: gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8



```

> Flag: gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8

> Level 24-25:

A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.\
You do not need to create new connections each time

```shell
cd /tmp/
vim brutef.sh

#!bin/bash

for i in {0000..9999}
do
	echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i" >> bandit25pin
done
#
cat bandit25pin | nc localhost 30002 >> bandit25pass 
cat bandit25pass
# tail -n 5 bandit25pass (muestra las 5 ultimas linias)
Output:
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```

```shell
'ALTERNATIVA'

#!/bin/bash
 
bandit24='gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8'
 
for i in {0..9}{0..9}{0..9}{0..9}
# for i in {0000..9999}
do
    echo $bandit24' '$i >> bandit25pin
done
cat ./bandit25pin | nc localhost 30002 >> ./bandit25pass
tail -n 5 ./bandit25pass
```

> Flag: iCi86ttT4KSNe1armKiwbQNmB3YJP3q4

> Level 25-26

Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not **/bin/bash**, but something else. Find out what it is, how it works and how to break out of it.

```bash
 # Para saber que tipo de shell tiene un usuario en linux:
getent passwd nombre_usuario

getent passwd bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext

# Analizamos el showtext:

#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0



```

Para lograr conectanos y escapar de more, tenemos que hacer mas perqueña la terminal, hasta que aparezca la restriccion de tamaño de more (--more--40%)

Despues, escapamos del editor, que ya esta corriendo como bandit26:

```shell
v
:e /etc/bandit_pass/bandit26
s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ

'Para lograr la shell como usuario bandit26'

:set shell=/bin/bash
:shell 
bandit26@bandit:

```

> Flag: s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ

> Level 26-27

```shell
'Encontramos en la shell de bandit 26 obtenida explotando more en el apartado anterior, un binario suid que permite ejecutar comandos siendo bandit27'

bandit26$: ./bandit27-do cat /etc/bandit_pass/bandit27

upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
```

> Flag : upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB

> Level 27-28:

There is a git repository at `ssh://bandit27-git@localhost/home/bandit27-git/repo` via the port `2220`. The password for the user `bandit27-git` is the same as for the user `bandit27`.

Clone the repository and find the password for the next level.

```shell

'Como en la maquina bandit no tenemos permiso de escritura, nos clonamos el repo en nuestra maquina cambiado el localhost por la de bandit'

git clone ssh://bandit27-git@localhost/home/bandit27-git/repo

# Cambiamos @localhost por:

git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo

cd repo
cat README

The password to the next level is: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
```

> Flag: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN

> Level 28-29:
>
> There is a git repository at `ssh://bandit28-git@localhost/home/bandit28-git/repo` via the port `2220`. The password for the user `bandit28-git` is the same as for the user `bandit28`.

Clone the repository and find the password for the next level.

```shell
'Parece lo mismo que el nivel anterior. Clonamos el repositorio y abrimos README'

sudo git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo

cd repo
cat README
Output:
# Bandit Notes
    Some notes for level29 of bandit.
    
    ## credentials
    
    - username: bandit29
    - password: xxxxxxxxxx

'Para ver los últimos cambios en el repositorio hacemos, dentro la carpeta repo:'

git show
Output:

commit 817e303aa6c2b207ea043c7bba1bb7575dc4ea73 (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Sep 19 07:08:39 2024 +0000

    fix info leak

diff --git a/README.md b/README.md
index d4e3b74..5c6457b 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials
 
 - username: bandit29
-- password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
+- password: xxxxxxxxxx


'Vemos el cambio en el campo password'
4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
```

> Flag: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7

> Level 29-30:

There is a git repository at `ssh://bandit29-git@localhost/home/bandit29-git/repo` via the port `2220`. The password for the user `bandit29-git` is the same as for the user `bandit29`.

Clone the repository and find the password for the next level.

```shell
cat README.md

- username: bandit30
- password: <no passwords in production!>

git show:

diff --git a/README.md b/README.md
index 2da2f39..1af21d3 100644
commit 6ac7796430c0f39290a0e29a4d32e5126544b022 (HEAD -> master, origin/master, origin/HEAD)

'Vemos que comit se hizo en cada linea'

git blame 

e65a928 (Ben Dover 2024-09-19 07:08:41 +0000 1) # Bandit Notes
^e65a928 (Ben Dover 2024-09-19 07:08:41 +0000 2) Some notes for bandit30 of bandit.
^e65a928 (Ben Dover 2024-09-19 07:08:41 +0000 3) 
^e65a928 (Ben Dover 2024-09-19 07:08:41 +0000 4) ## credentials
^e65a928 (Ben Dover 2024-09-19 07:08:41 +0000 5) 
6ac77964 (Ben Dover 2024-09-19 07:08:41 +0000 6) - username: bandit30
^e65a928 (Ben Dover 2024-09-19 07:08:41 +0000 7) - password: <no passwords in production!>
^e65a928 (Ben Dover 2024-09-19 07:08:41 +0000 8) 

'No vemos nada'

'Probamos a listar todas las ramas, incluyend las remotas'

git branch -a

HEAD desacoplado en origin/master)
  master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
#Aparecen las ramas que pueden coincidir con las versiones en develope (no produccion)

git show /remotes/origin/dev

 - username: bandit30
-- password: <no passwords in production!>
+- password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
```

> Flag: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL

> Level 30-31

There is a git repository at `ssh://bandit30-git@localhost/home/bandit30-git/repo` via the port `2220`. The password for the user `bandit30-git` is the same as for the user `bandit30`.

Clone the repository and find the password for the next level.

```shell
'Hacemos lo mismo que en los casos anteriores'
cat README.md
 just an epmty file... muahaha
git show

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..029ba42
--- /dev/null
+++ b/README.md
#Esto nos indica que el fichero READM.md no existia previamente y se ha creado en blanco

git branch -a # Buscamos ramas remotas, pero no aparece nada
git tab # Buscamos etiquetas o ramas antiguas
secret
#buscamos modificaciones anteriores a la etiqueta secret
git show secret
fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
```

> Flag: fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy

> Level 31-32

There is a git repository at `ssh://bandit31-git@localhost/home/bandit31-git/repo` via the port `2220`. The password for the user `bandit31-git` is the same as for the user `bandit31`.

Clone the repository and find the password for the next level.

```shell

cat README.md
Output:
This time your task is to push a file to the remote repository.
   Details:
   File name: key.txt
   Content: 'May I come in?'
   Branch: master

'creamos un fichero llamado key.txt con el contenido indicado'

git add -f key.txt #-f para forzar a incluir el archivo 
git commit -m "Agregando key.txt"
#Error: identidad del autor desconocido
#*** Por favor cuéntame quién eres.
#Ejecuta
#  git config --global user.email #"you@example.com"
#  git config --global user.name "Tu Nombre"
git config user.name "name"
git config user.mail "mail"
git commit -m "Agregando key.txt"
 1 file changed, 1 insertion(+)
 create mode 100644 key.txt
git push origin master
output:

remote: Well done! Here is the password for the next level:
remote: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K 
```

> Flag: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K

> Level 32-33

After all this `git` stuff, it’s time for another escape. Good luck!

````shell
'Al conectar encontramos una shell sh que pone todo lo que escribimos en mayusculas
Para escapar de la sh-1 hacemos'
>$0
# como el binario upppershell tiene el bit suid para bandit32, lo usamos para ejecutar la shell como bandit33
./uppershell bandit33
$0 # escapamos de nuevo

# leemos el contenido de bandit33
cat /etc/bandit_pass/bandit33

tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0```
>Flag: tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0
````

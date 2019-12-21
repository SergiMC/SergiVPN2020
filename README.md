# SergiVPN2020
Treball de VPN 2020 Sergi Muñoz Carmona

## Creació de VPN amb TLS utilitzant AWS

Crearem una VPN amb l'estructura:
* Server (Configurarem el server en una màquina AWS).
* Client 1 (Configurarem el client a un host).
* Client 2 (Configurarem el client a un host).

## Creació de claus i certificació

Utilitzarem openssl per crear les claus i la certificació (CA) per crear els túnels VPN.
L'entitat que crearem s'anomenarà: **Sergi2020** que generarà els certificats del server i dels clients.

## Server
Configurarem el servidor amb els certificats i claus:

* clau privada i certificat de CA
* clau privada i certificat de servidor

## Clients 1 i 2
Configurarem els clients amb els certificats i claus:

* clau privada i certificat de client


## Creació de claus i certificats

* **Servidor**.
Per a la creació d'un autocertificat, hem de generar com a primera instància la clau privada.

```[root@ip-172-31-30-207 server]# openssl genrsa -out cakey.pem
Generating RSA private key, 2048 bit long modulus
.......................................+++++
...+++++
e is 65537 (0x010001)
```

Obtinguda la clau privada, generarem el CA anomenat com Sergi2020.

```
[root@ip-172-31-30-207 server]# openssl req -new -x509 -nodes -key cakey.pem -days 4650 -out cacert.pem
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:SP
State or Province Name (full name) []:Barcelona
Locality Name (eg, city) [Default City]:Barcelona
Organization Name (eg, company) [Default Company Ltd]:Sergi2020
Organizational Unit Name (eg, section) []:Departament de sergi
Common Name (eg, your name or your server's hostname) []:Sergi2020
Email Address []:admin@sergi2020.org
[root@ip-172-31-30-207 server]# openssl genrsa -out serverkey.pem
Generating RSA private key, 2048 bit long modulus
..............................................................+++++
.......................+++++
e is 65537 (0x010001)
```

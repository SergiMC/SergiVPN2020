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

* **SERVIDOR**

Per a la creació d'un autocertificat, hem de generar com a primera instància la clau privada.

```
[root@ip-172-31-30-207 server]# openssl genrsa -out serverkey.pem
Generating RSA private key, 2048 bit long modulus
..............................................................+++++
.......................+++++
e is 65537 (0x010001)
```

Obtinguda la clau privada, generarem el request.

```
[root@ip-172-31-30-207 server]# openssl req -new -key serverkey.pem -out serverreq.pem
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

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

Per finalitzar, crearem la CA del servidor anomenada Sergi2020.

```
[root@ip-172-31-30-207 server]# openssl x509 -CA cacert.pem -CAkey cakey.pem -req -in serverreq.pem -days 3650 -extfile ca.server.conf -CAcreateserial -out servercert.pem
Signature ok
subject=C = SP, ST = Barcelona, L = Barcelona, O = Sergi2020, OU = Departament de sergi, CN = Sergi2020, emailAddress = admin@sergi2020.org
Getting CA Private Key
```

* **CLIENT1** 

Per a la creació d'un autocertificat, hem de generar com a primera instància la clau privada.

```
[root@192 client1]# openssl genrsa -out client1key.pem
Generating RSA private key, 2048 bit long modulus
.........+++
...........................+++
e is 65537 (0x010001)
```

Obtinguda la clau privada, generarem el request.

```
[root@192 client1]# openssl req -new -key client1key.pem -out client1req.pem
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

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:

```
Per finalitzar, crearem la CA del client1 anomenada Sergi2020.

```
[root@192 client1]# openssl x509 -CA cacert.pem -CAkey cakey.pem -req -in client1req.pem -days 3650 -extfile ca.client.conf -CAcreateserial -out client1cert.pem
Signature ok
subject=C = SP, ST = Barcelona, L = Barcelona, O = Sergi2020, OU = Departament de sergi, CN = Sergi2020, emailAddress = admin@sergi2020.org
Getting CA Private Key
```

* **CLIENT2** 

Per a la creació d'un autocertificat, hem de generar com a primera instància la clau privada.

```
[root@192 client2]# openssl genrsa -out client2key.pem
Generating RSA private key, 2048 bit long modulus
..................................................+++
.................+++
e is 65537 (0x010001)
```

Obtinguda la clau privada, generarem el request.

```
[root@192 client2]# openssl req -new -key client2key.pem -out client2req.pem
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

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

Per finalitzar, crearem la CA del client1 anomenada Sergi2020.

```
[root@192 client2]# openssl x509 -CA cacert.pem -CAkey cakey.pem -req -in client2req.pem -days 3650 -extfile ca.client.conf -CAcreateserial -out client2cert.pem
Signature ok
subject=C = SP, ST = Barcelona, L = Barcelona, O = Sergi2020, OU = Departament de sergi, CN = Sergi2020, emailAddress = admin@sergi2020.org
Getting CA Private Key
```

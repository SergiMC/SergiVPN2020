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

## **Activació dels serveis**

* **Activació del servidor**

```
[root@ip-172-31-30-207 fedora]# systemctl start openvpn-server@server.service
[root@ip-172-31-30-207 fedora]# systemctl status openvpn-server@server.service
● openvpn-server@server.service - OpenVPN service for server
   Loaded: loaded (/usr/lib/systemd/system/openvpn-server@.service; disabled; v>
   Active: active (running) since Mon 2019-12-23 12:38:28 UTC; 4s ago
     Docs: man:openvpn(8)
           https://community.openvpn.net/openvpn/wiki/Openvpn24ManPage
           https://community.openvpn.net/openvpn/wiki/HOWTO
 Main PID: 939 (openvpn)
   Status: "Initialization Sequence Completed"
    Tasks: 1 (limit: 1141)
   Memory: 2.6M
   CGroup: /system.slice/system-openvpn\x2dserver.slice/openvpn-server@server.s>
           └─939 /usr/sbin/openvpn --status /run/openvpn-server/status-server.l>

```

* **Activació del client**

```
[root@192 client]# systemctl start openvpn-client@client.service
[root@192 client]# systemctl status openvpn-client@client.service
● openvpn-client@client.service - OpenVPN tunnel for client
   Loaded: loaded (/usr/lib/systemd/system/openvpn-client@.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2019-12-23 13:41:19 CET; 5s ago
     Docs: man:openvpn(8)
           https://community.openvpn.net/openvpn/wiki/Openvpn24ManPage
           https://community.openvpn.net/openvpn/wiki/HOWTO
 Main PID: 4665 (openvpn)
   Status: "Initialization Sequence Completed"
    Tasks: 1 (limit: 4915)
   CGroup: /system.slice/system-openvpn\x2dclient.slice/openvpn-client@client.service
           └─4665 /usr/sbin/openvpn --suppress-timestamps --nobind --config client.conf

Dec 23 13:41:20 192.168.1.139 openvpn[4665]: Incoming Data Channel: Cipher 'AES-256-GCM' initialized with 256 bit key
Dec 23 13:41:20 192.168.1.139 openvpn[4665]: ROUTE_GATEWAY 192.168.1.1/255.255.255.0 IFACE=wlp0s29u1u2 HWADDR=14:cc:20:22:5f:e7
Dec 23 13:41:20 192.168.1.139 openvpn[4665]: TUN/TAP device tun0 opened
Dec 23 13:41:20 192.168.1.139 openvpn[4665]: TUN/TAP TX queue length set to 100
Dec 23 13:41:20 192.168.1.139 openvpn[4665]: do_ifconfig, tt->did_ifconfig_ipv6_setup=0
Dec 23 13:41:20 192.168.1.139 openvpn[4665]: /sbin/ip link set dev tun0 up mtu 1500
Dec 23 13:41:20 192.168.1.139 openvpn[4665]: /sbin/ip addr add dev tun0 local 10.8.0.6 peer 10.8.0.5
Dec 23 13:41:20 192.168.1.139 openvpn[4665]: /sbin/ip route add 10.8.0.0/24 via 10.8.0.5
Dec 23 13:41:20 192.168.1.139 openvpn[4665]: WARNING: this configuration may cache passwords in memory -- use the auth-nocache option to prevent this
Dec 23 13:41:20 192.168.1.139 openvpn[4665]: Initialization Sequence Completed

```

## **Extensions dels certificats**

* **Extensió del certificat del servidor**
```            
[sergimc@192 server]$ openssl x509 -noout -text -in servercert.pem

Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints: 
                CA:FALSE
            Netscape Cert Type: 
                SSL Server
            Netscape Comment: 
                OpenSSL Generated Server Certificate
            X509v3 Subject Key Identifier: 
                72:6B:67:DC:D5:49:20:53:07:13:7A:62:6D:C0:98:E0:7F:AC:A0:3A
            X509v3 Authority Key Identifier: 
                keyid:A1:3B:82:93:C4:EB:78:45:8E:CE:DC:C0:41:AB:32:50:98:BC:CF:BB
                DirName:/C=SP/ST=Barcelona/L=Barcelona/O=Sergi2020/OU=Departament de sergi/CN=Sergi2020/emailAddress=admin@sergi2020.org
                serial:99:22:AD:31:97:68:D9:EA

            X509v3 Extended Key Usage: 
                TLS Web Server Authentication
            X509v3 Key Usage: 
                Digital Signature, Key Encipherment
    Signature Algorithm: sha256WithRSAEncryption

```

* **Extensio del certificat del client1**


```
[sergimc@192 client1]$ openssl x509 -noout -text -in client1cert.pem 

    X509v3 extensions:
            X509v3 Basic Constraints: 
                CA:FALSE
            X509v3 Subject Key Identifier: 
                38:D5:A7:FC:89:C6:1E:52:6F:19:5D:72:F0:95:95:AD:99:10:EC:5C
            X509v3 Authority Key Identifier: 
                keyid:A1:3B:82:93:C4:EB:78:45:8E:CE:DC:C0:41:AB:32:50:98:BC:CF:BB
                DirName:/C=SP/ST=Barcelona/L=Barcelona/O=Sergi2020/OU=Departament de sergi/CN=Sergi2020/emailAddress=admin@sergi2020.org
                serial:99:22:AD:31:97:68:D9:EA

    Signature Algorithm: sha256WithRSAEncryption
```

* **Extensió del certificat del client2**

```
[sergimc@192 client2]$ openssl x509 -noout -text -in client2cert.pem 

 X509v3 extensions:
            X509v3 Basic Constraints: 
                CA:FALSE
            X509v3 Subject Key Identifier: 
                B2:27:EC:65:FF:91:16:05:AD:71:E5:4F:1C:32:FD:B5:63:BF:3F:B4
            X509v3 Authority Key Identifier: 
                keyid:A1:3B:82:93:C4:EB:78:45:8E:CE:DC:C0:41:AB:32:50:98:BC:CF:BB
                DirName:/C=SP/ST=Barcelona/L=Barcelona/O=Sergi2020/OU=Departament de sergi/CN=Sergi2020/emailAddress=admin@sergi2020.org
                serial:99:22:AD:31:97:68:D9:EA

    Signature Algorithm: sha256WithRSAEncryption
```

## **Comprovació del tunel**

Per comprovar que el tunel funciona correctament, utilitzarem la comanda ip a per confirmar que els tunels estàn creats correctament i ping per confirmar el seu funcionament.

* **Ip a**

ip a per part del servidor.
```
[root@ip-172-31-30-207 fedora]# ip a

3: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UNKNOWN group default qlen 100
    link/none 
    inet 10.8.0.1 peer 10.8.0.2/32 scope global tun0
       valid_lft forever preferred_lft forever
    inet6 fe80::ce11:f6cd:6fcd:8c14/64 scope link stable-privacy 
       valid_lft forever preferred_lft forever


```
ip a per part del client .
```
[root@192 client]# ip a

14: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UNKNOWN group default qlen 100
    link/none 
    inet 10.8.0.6 peer 10.8.0.5/32 scope global tun0
       valid_lft forever preferred_lft forever
    inet6 fe80::c4ff:2caf:e0d1:81c5/64 scope link flags 800 
       valid_lft forever preferred_lft forever

```
* **Ping**

ping del servidor al client.
```
[root@ip-172-31-30-207 fedora]# ping 10.8.0.6
PING 10.8.0.6 (10.8.0.6) 56(84) bytes of data.
64 bytes from 10.8.0.6: icmp_seq=1 ttl=64 time=34.8 ms
64 bytes from 10.8.0.6: icmp_seq=2 ttl=64 time=34.7 ms
64 bytes from 10.8.0.6: icmp_seq=3 ttl=64 time=39.2 ms
64 bytes from 10.8.0.6: icmp_seq=4 ttl=64 time=75.7 ms
^C
--- 10.8.0.6 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 34.798/46.177/75.780/17.187 ms

```

ping del client al servidor.
```
[root@192 client]# ping 10.8.0.1
PING 10.8.0.1 (10.8.0.1) 56(84) bytes of data.
64 bytes from 10.8.0.1: icmp_seq=1 ttl=64 time=35.0 ms
64 bytes from 10.8.0.1: icmp_seq=2 ttl=64 time=48.4 ms
64 bytes from 10.8.0.1: icmp_seq=3 ttl=64 time=35.5 ms
64 bytes from 10.8.0.1: icmp_seq=4 ttl=64 time=35.1 ms

```

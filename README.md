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

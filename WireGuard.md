WireGuard es una VPN liviana, y de fácil configuración, que permite encapsular y encriptar paquetes IP sobre UDP. Encripta la información a nivel L3.

Podemos utilizar WG en conjunto con protocolos GRE o EoIP para, primero, crear un túnel entre sitios (usando GRE o EoIP) que permitan *multicast* y, segundo, poder encriptar la información que viaja por ese túnel (Usando WG).

## Configuración en Winbox

Lo primero que debemos hacer es crear la interfaz
![[interfaz_WG.png]]
Ahora copiamos la "public key" que la usaremos para emparejarnos con el *router* con el cual queremos crear el túnel.
Luego, creamos la interfaz virtual que será usada por WG y le asignamos una dirección IP.

Ya terminado eso, debemos ir a la pestaña de "peer" para ingresar la información del 
*router* con quien nos emparejaremos. Elegimos la interfaz  y configuramos el "Allowed Addresses", 
Este campo especifica la lista de direcciones IP de origen que son permitidas viniendo desde el *peer*, y que tráfico de destino será dirigido al *peer*. Se puede explicara de otra manera que funciona como un tipo de tabla de *routeo* cuando envía paquetes, y como ACL cuando recibe paquetes.

En "Public Key" pegamos la clave pública de quien será nuestro vecino WG
El "Persisten Keepalive" se usa para mantener el túnel activo en caso de que no haya tráfico.
En "Endpoint" ponemos la dirección IP o el *hostname*. WWG lo usa para establecer la conexión entre los 2 *peers*. Al menos 1 de los 2 *peers* debe tener el *endpoint* para iniciar la conexión
	"Endpoint port" Es el puerto UDP

Ahora debemos realizar estos mismos pasos en el otro equipo que formará parte del túnel WG.
Ahora pegamos las claves públicas de cada equipo en la pestaña de "peers" del otro.

Recordar que para que el túnel funcione, debemos tener una ruta hacia nuestro *peer*.
También recordar crear reglas en el FW para permitir el tráfico.


[Más info](https://help.mikrotik.com/docs/spaces/ROS/pages/69664792/WireGuard)

Más info todavía:

WireGuard associates tunnel IP addresses with public keys and remote endpoints. When the interface sends a packet to a peer, it does the following:

1. This packet is meant for 192.168.30.8. Which peer is that? Let me look... Okay, it's for peer `ABCDEFGH`. (Or if it's not for any configured peer, drop the packet.)
2. Encrypt entire IP packet using peer `ABCDEFGH`'s public key.
3. What is the remote endpoint of peer `ABCDEFGH`? Let me look... Okay, the endpoint is UDP port 53133 on host 216.58.211.110.
4. Send encrypted bytes from step 2 over the Internet to 216.58.211.110:53133 using UDP.

When the interface receives a packet, this happens:

1. I just got a packet from UDP port 7361 on host 98.139.183.24. Let's decrypt it!
2. It decrypted and authenticated properly for peer `LMNOPQRS`. Okay, let's remember that peer `LMNOPQRS`'s most recent Internet endpoint is 98.139.183.24:7361 using UDP.
3. Once decrypted, the plain-text packet is from 192.168.43.89. Is peer `LMNOPQRS` allowed to be sending us packets as 192.168.43.89?
4. If so, accept the packet on the interface. If not, drop it.
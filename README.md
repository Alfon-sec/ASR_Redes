# ASR_Redes



## 6.1.1: Arquitectura de comunicaciones en red: TCP/IP
La ultima versión es NET - 4 

Se requiere identificar al sistema (tarjeta de RED) dentro de la red mediante IP 

### Niveles
 - Físico => cable
 
 	Se intercambian bits
 - Enlace (control de lineas o acceso a red)=> ethernet (PPP...) => la identificacion es la #MAC:
 
 	Se intercambian tramas: { #Dest|#Src| ... |CRC }
	
	Se realizan control de errores (CRC: Solo lo detecta, NO LO CORRIGE => un protocolo de mas nivel lo corregira), control de flujos (Permite al receptor controlar si debe transmitir o no (Para no congestionar))
	
 - Interred => IP (ICMP...)
 	
	@IP: se podria decir, que esta asociada al software
	
	Datagrama (Tiene un campo para identificar el protocolo. En linux se puede obtener la lista de protocolos en el directorio ```/etc/protocol```)
	
	Best effort: No hace ningun control de errores, ni control de nada. Pueden llegar datagramas duplicados, o que no lleguen...
 
 - Transporte => TCP (UDP...)
 
   Segmentos
  
   Aporta fiabilidad al nivel de Interred => TCP aunque no siempre es necesario => UDP
 
 
 - Aplicación => HTTP, DNS, SSH ... (modelo cliente/servidor)
 
   Identificar al proceso: Puerto (UDP tiene asociado unos puertos y TCP tiene otros)
   
   En el fichero ```/etc/services``` se encuentra asociados varios servicios con el protocolo y puerto 
   
   
   ARP asocia una @IP con una #MAC
   
   Un usuario, para acceder a una aplicacion necesita conocer una IP o un nombre asociada a una IP (la transoformacion la hace un serivdor de nombres DNS)
   
CONCEPTOS:
  - Protocolo: comunicacion entre 2 sistemas en el mismo nivel
  - 
  - Interfaz: Medio para comunicar los 
  
  
### Tipos de redes

  - Red Local (LAN): Ethernet (IEEE 802.3), WIFI (IEEE 802.11) ... Direcciones MAC
  
  - Interred IP (Internet). Direcciones IP
  
       - Dirección + máscara (CIDR) [<= Clases (A,B,C)]. DHCP (asignar una configuracion de red a un sistema ).
       
       - Conexiones externas a la red local => pasarela (o gateway). Tabla de encaminamiento. En Windows con el comando ```route print```  o en Linux ```route``` imprimie la tabla de encaminamiento
       
       - Servidor de nombres (DNS)
       

### Direcciones IP
   
   - Tipos :
        - IPv4 vs IPv6:
		En IPv6 no se permite trocear un paquete, no se hace checksum. Incluye cifrado por defecto
        - Clases (obsoleto) A: 1-127, B: 128-191, C: 192-223, (D y E)
        - Pública vs Privada
            - Direcciones privadas: 10/8, 172.16/12, 192.168/16
            - Direcciones especializadas: localhost 127/8, auto-configuration 169.254/16 (la ip que se pone si no se encuentra un servidor DCHP, se auto asigna una)
   - Opciones:
        - IP masquerade (enmascarada) - NAT y NAT PT (port translation) => Establecer entre direcciones de red a otras (De publica a privada, de privada a privada, de publica a publica, ...)
        - Asignación: estática / dinámica (DHCP)
        - Traducción: DNS (named) / ARP - RARP

## 6.1.2: Configuración de red

### Configuracion

   - Opciones al asignar direcciones IP:
   
        - Estático 
	
		Ventajas: esa direccion no se puede reasignar, aunque el PC esté apgada. Un servidor tiene sentido
		
        - Dinámico: por medio de dhcp
		Ventajas: con un reducido numero de IPs puedes atender a varios hosts. Ademas permite una configuracion de los hosts de manera automaica
		
   - Parámetros configurables:
        - Máscara
		
		Todas las maquinas de la misma red tienen la misma mascara 
		
        - Dirección broadcast
		
		La parte de la IP correspondiente al identificador con todo unos (192.168.255.255/16) envia a todos los hosts de la red corresponde con la mac FF:FF:FF:FF:FF:FF:FF:FF:FF.
		
        - Servidor DNS
	
	
        - Pasarela (gateway)
	
		
### Servicios

- Basados en puertos (binding)

	Los puertos no se asignan de manera random. Hay algunos puertos reservados

    - 0-65535 (TCP, UDP generan todos esos puertos. Todos los protocolos de la capa de transporte tienen un conjunto de puertos)

    	- 0-1023 (well-known): en modo superusuario
	- 1024-49151: user ports. los usuarios pueden registrar
	- 49151-FFFFx: system ports
    
- Fichero ```/etc/services```:
```
ssh        22/tcp                # SSH Remote Login Protocol
ssh        22/udp
telnet     23/tcp
smtp       25/tcp    mail
time       37/tcp   timserver
time       37/udp   timserver
...
www        80/tcp   http         # WorldWideWeb HTTP
www        80/udp                # HyperText Transfer Protocol
...
```
### Protocolos


   - Fichero ```/etc/protocols```:
```
    ip          0    IP          # internet protocol, pseudo protocol number
    #hopopt     0    HOPOPT      # IPv6 Hop-by-Hop Option [RFC1883]
    icmp        1    ICMP        # internet control message protocol
    igmp        2    IGMP        # Internet Group Management
    ggp         3    GGP         # gateway-gateway protocol
    ipencap     4    IP-ENCAP    # IP encapsulated in IP (officially IP)
    st          5    ST          # ST datagram mode
    tcp         6    TCP         # transmission control protocol
    ...
```

### Nombres => direcciones IP

   - Local: ```/etc/hosts```:
   
   	Son valores estaticos
```
    127.0.0.1    localhost
    127.0.1.1    etch
    ...
```

   - Servidor (DNS): ```/etc/resolv.conf```:
   
   	Se especifica el servidor DNS local
```
    nameserver 168.95.1.1
    nameserver 168.95.1.2
    ...
```

### Comandos básicos 
- Herramientas de redes y control de tráfico:

    - paquete net-tools: arp, ifconfig, netstat, route ...
    	
		conjunto  de herramientas de red
		arp: conunjunto de re redes en nuestra red
		ifconfig: configurar la configuracion de red
		netstat: network statistics
		route: tabla de reenvio
		
    - paquete iproute2: ip, ss ...
    		
		es una version mejorada del paquete net-tools

- Configuración del interfaz y prueba: ```ifconfig```

- Tráfico local: ```netstat -i```

- Servicios abiertos: ```netstat -ta``` (o ```-ua``` para UDP)

- Direcciones ethernet conocidas: ```arp -a```

- Tabla de reenvío: configuración y prueba ```route```,``` netstat -rn```

- Monitorizar tráfico TCP/IP: ```ping/traceroute```,``` mtr```,``` iptraf```

- Otras: ```ipcalc```, ```ethtool```,

	net-tools 	|iproute2
	----------------|--------------
	arp -na 	|ip neigh
	ifconfig -a 	|ip addr
	ifconfig –help 	|ip help
	ifconfig eth0 up|ip link set eth0 up
	netstat 	|ss
	netstat -i 	|ip -s link
	netstat -tulpan |ss -tulpan
	route 		|ip route
	route add 	|ip route add
	
	
![alt text](https://egela1819.ehu.eus/pluginfile.php/1358215/mod_resource/content/121/html/_images/net-tools_vs_iproute2.png)


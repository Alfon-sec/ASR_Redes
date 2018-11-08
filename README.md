# ASR_Redes



## 6.1: Protocolo de red TCP/IP
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
       - Dirección + máscara (CIDR) [<= Clases (A,B,C)]. DHCP.
       - Conexiones externas a la red local; pasarela (o gateway). Tabla de encaminamiento.
       - Servidor de nombres (DNS)


  

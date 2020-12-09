# Wireshark 

Security, Owasp top 10, Cron. (#nmap, #samba, #suid, #pathvarmanipulation #smb)

## Tabla de contenido

- [Introducción](#Introducción).
- [Ejemplos de TCP reensamblando la data](#Ejemplos-de-TCP-reensamblando-la-data).
- [Tipos de escaneo - Descripción general](#Tipos-de-escaneo---Descripción-general).


--------------------------------
### Introducción
-------------------------------

¿Qué es Wireshark?

* Wireshark tiene capturar todos los paquetes enviados y recibidos a tráves del adaptador de red.

--------------------------------
### Ejemplos de TCP reensamblando la data
-------------------------------

1) No reassembly required
    
    Cuando el **servidor** recibe la data se envía en el mismo orden del **cliente**:

    ```
    1) Sent 1st - Received 1st
    2) Sent 2nd - Received 2nd
    3) Sent 3rd - Received 3rd
    ```

2) Reassembly required

    Cuando el **servidor** recive toda la data pero en diferente orden, reordenar es requerido:
  
    ```
    1) Sent 1st - Received 1st
    2) Sent 2nd - Received 3rd
    3) Sent 3rd - Received 2nd
    ```

3) The connection is dropped
    
    Si el cliente envía 3 paquetes pero sólo llegan 2, la data termina corrupta.

    ```
    1) Sent 1st - Received 1st
    2) Sent 2nd - Not received
    3) Sent 3rd - Received 2nd
    ```


--------------------------------
### Filtros más utilizados
-------------------------------

|Filtro       | Descripción    | Ejemplo  |
| :---        |     :---:      |   :---:  | 
| ip.src	  | Muestra todos los paquetes que son envíados de una IP específica    |  ip.src == 192.168.1.1
| ip.dst	  | Muestra todos los paquetes son destinado a una IP específica    | ip.dst == 192.168.1.1
|tcp/udp.port | Muestra todos los paquetes que son envíados vía un protocolo y puerto específico | tcp.port == 22 / udp.port == 67
|protocol.request.method | Muestra todos los paquetes que son envíados a través de los métodos GET, POST, etc. | http.request.method == GET / POST
.

--------------------------------
### Combinando filtros 
-------------------------------

|Operador       | Descripción    | Ejemplo  |
| :---        |     :---:      |   :---:  | 
| ==	  | Operador para verificar si el filtro coincide exactamente con el valor dado en todos los paquetes.    |  ip.addr == 192.168.1.1
| !=	  | Operador comprueba si el filtro no coincide con el valor dado.    | ip.addr != 192.168.1.10
|&&	      | Utilice este operador para combinar varios filtros juntos. | ip.addr == 192.168.1.1 && ip.addr == 192.168.1.10
.


--------------------------------
### Herramienta con la que combina de maravilla 
-------------------------------

NewtworkMiner es un analizador forense de red que se puede utilizar para detectar el sistema operativo, el nombre de host, las sesiones y los puertos abiertos a través de la práctica sniffing de paquete o mediante un archivo PCAP.
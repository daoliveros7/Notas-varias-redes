### HABILITAR CONEXIONES ENTRE VMS CON GNS3 Y PNETLAB EN LINUX ###

Este documento describe los pasos a tener en cuenta si estás usando
Debian 13 u otro sistema linux nativo, y estás usando VMWare Workstation
con una VM como GNS3 o PNETLAB para emular topologías de red.

Un problema que se tiene aquí es que debido a la estructura del sistema
operativo y de los permisos de Linux, las conexiones entre máquinas virtuales
hacia GNS3 VM o PNETLAB a través de Host-Only, Bridge, Segmento LAN, son 
restringidas ya que el modo promiscuo no se puede habilitar en este SO con 
facilidad. Entonces notaremos que los ping, traceroute u otros intentos de 
conexion fallan, inclusive aunque la VM reciba direccionamiento IP por DHCP
como se espera.

Por ello, se describen los pasos realizados para solucionar esta falla.

#### Solucion #####3

Paso 1

Apague completamente todas las maquinas virtuales de VMWare

inicie sesion en una terminal como root o utilice sudo su.

Configure los permisos de los adaptadores vmnet que necesite de la siguiente 
forma:

chmod 666 /dev/vmnet*

; esto asigna permisos de lectura y escritura para todos los usuarios en 
; todos los adaptdores vmnet que tenga. puede usarlo solo para el vmnet
; específico que necesite.

Paso 2

Busque la ruta donde están las máquinas virtuales que desea conectar a 
traves de nubes. (ej: GNS3 VM con una VM Windows)
Abra el archivo .vmx de cada máquina virtual que desee conectar.
Identifique los nombres de las tarjetas de red con las que cuenta la VM
(usualmente suelen llamarse ethernet1, ethernet2, ...)

Al final del ficero .vmx de cada máquina, agregue las siguientes lineas de 
acuerdo con la cantidad de tarjetas de red que desea conectar en cada máquina:

ethernetX.noPromisc = "FALSE"

# ejemplo (tengo dos tarjetas en mi GNS3 VM: eth2 y eth3 a las que 
# deshabilitaré el modo promiscuo)

nano "/home/daoliveros7/vmware/GNS3 VM/GNS3 VM.vmx"
nano /home/daoliveros7/vmware/GNS3\ VM/GNS3\ VM.vmx

# agrega al final
ethernet2.noPromisc = "FALSE"
ethernet3.noPromisc = "FALSE"

# repito los pasos editando los ficheros vmx de TODAS las máquinas virtuales
# que necesite conectar a la topología de GNS3 o PNETLAB.

Paso 3

Como root, reinicio las interfaces de vmware con los siguientes comandos:

vmware-networks --stop
vmware-networks --start

Paso 4

Encienda las maquinas virtuales, conectelas de acuerdo con sus necesidades
dentro de la topología en GNS3 o PNETLAB.

Verifique la correcta conectividad entre las VMs con los appliances de la 
topología!

# De esta forma, se pueden conectar las maquinas virtuales sin problema con 
# las topologías simuladas en GNS3 y PNETLAB. 

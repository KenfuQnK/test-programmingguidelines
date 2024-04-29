---
draft: false
---


Con el fin de poder compartir rutinas de forma más practica, y de reducir la longitud de los tags, se debe referenciar las estructuras mayores más habituales usando los ``Alias``

# 01. Máquina

| mod1XXX_Machine. | mach  |
| ---------------- | ----- |
| .Command         | cmd   |
| .Status          | sts   |
| .Parameters      | param |
| .Values          | val   |
| .Devices         | dev   |
| .Alarms          | alm   |
| .Production      | prod  |
| .Counters        | count |
| .Operation       | oper  |
| .Mode            | mode  |
| .Services        | serv  |
| .Register        | reg   |
| .PanelPCX        |       |
| .Doors           |       |

Se pueden hacer alias de alias

>[!example]+ Ejemplo
>Al hacer un alias ``st`` del GlobalTag, el resto de elementos de esa estructura pueden usar el ``st`` y de esta forma solo hay que actualizar un alias en la estación 
>![[Pasted image 20240426175234.png]]


![[Pasted image 20240426180601.png]]


# 02. Estaciones




| mod1XXX_stYYYYY. | st    |
| ---------------- | ----- |
| .Command         | cmd   |
| .Status          | sts   |
| .Parameters      | param |
| .Values          | val   |
| .Devices         | dev   |
| .Mode            | mode  |
| .Sequence        | seq   |

mod1XXX_stYYYYY.
- Mode
- Enable
- Inhibit
- ``InhibitStation1``
- ``InhibitStation2``
- ``RejectWhenNoPresence``

Los modos de funcionamiento de la máquina activarán/desactivaran los enable de las estaciones.

Ej: El modo vaciado desactivará el Enable de la estación Infeed

•

De forma manual también se pueden desactivar, pero los elementos tendrán que estar en una posición de reposo segura

El inhibit solo se puede activar/desactivar de forma manual, por si por ejemplo se estropea un motor y no se puede poner en posición pero aun así se quiere que la máquina trabaje.

Con el inhibit activado, las alarmas asociadas no deben aparecer









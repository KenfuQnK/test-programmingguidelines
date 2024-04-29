---
draft: false
---


## 1. Segmentación máquina

La nueva estructura de máquina segmenta la misma en tres principales partes:
- Operación
- Módulos (ciclos)
- Estaciones

Tal y como muestra la siguiente imagen:

## 2. Secuencias control

## 2.1. Proceso Lote

Diagrama estados

La siguiente tabla enumera los cinco estados del proceso de lote y la implementación detallada para aclarar la implementación con la intervención del Operador según corresponda para clarificarlo.

| **Nombre Estado** | **Tipo** | **Implementación detallada**                                                                                                                                                                                                                                                        |
| ----------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| DESHABILITADO     | Espera   | Resultado de terminar el estado FINISHED o por petición de abortar. El lote queda inactivo en este estado.<br><br>En esta transición se realizan:<br>- Desactivación históricos zenon.<br>- Reset datos lote en PLC                                                                 |
| ACTIVANDO         | Activo   | Como resultado de petición de inicio de lote, para transicionar a ACTIVATE.<br><br>En esta transición se realizan:<br><br>-       Carga datos del lote, tanto en PLC como en zenon<br>-       Reset de contadores y registro desplazamiento<br>-       Activación históricos zenon. |
| ACTIVO            | Espera   | Resultado de terminar el estado de transición ACTIVATING.<br><br>El lote queda activado.<br><br>Dentro del este estado se puede efectuar el vaciado de máquina. Sin necesidad de finalizar el lote.                                                                                 |
| FINALIZANDO       | Activo   | Como resultado de petición de finalizar lote para transicionar a FINISHED.<br><br>En esta transición se realizan:<br>-       Desactivación históricos zenon<br>-       Reset datos lote en PLC                                                                                      |
| FINALIZADO        | Espera   | Resultado de terminar el estado FINISHING. Lote finalizado.                                                                                                                                                                                                                         |

## 2.2. Operación Máquina

La secuencia de operación de máquina, es la secuencia principal que controla el funcionamiento de la máquina.

La secuencia se encarga de recibir las peticiones externas de arranque o paro, así como de recibir las peticiones de paro o deshabilitación por motivo de algún fallo.

La operación de máquina envía las diferentes peticiones de cambio de estado a los módulos de la máquina (ciclos) y espera la transición de los mismos al estado de espera solicitado.

Diagrama estados

La siguiente tabla enumera los siete estados del modelo y la implementación detallada para aclarar la implementación con la intervención del Operador según corresponda para clarificarlo.

| **Estado**         | **Descripción**                                                                                                                                                                                                                                                                                                                | **Implementación detallada**                                                                                                                                            |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **HABILITADO**     | **Tipo Estado: Espera**<br><br>La máquina está encendida y posicionada después de completar el estado de HABILITACIÓN O DE PARADA. Todas las comunicaciones con otros sistemas están funcionando (si corresponde).                                                                                                             | La máquina ha sido “re-posicionada” correctamente  y está esperando la solicitud de ARRANQUE.                                                                           |
| **INICIANDO**      | **Tipo Estado: Activo**<br><br>La máquina completa los pasos necesarios para comenzar. Este estado se ingresa como resultado de un comando de ARRANQUE (local). Siguiendo este comando, la máquina comenzará a ponerse en marcha.                                                                                              | Petición de ARRANQUE desde el estado ENABLED.                                                                                                                           |
| **EN MARCHA**      | **Tipo Estado: Activo**<br><br>Una vez que la máquina está procesando materiales, se encuentra en el estado RUNNING.                                                                                                                                                                                                           | Ejecución por una solicitud de ARRANQUE desde el estado HABILITADO. La máquina está produciendo producto a la velocidad de producción deseada                           |
| **HABILITANDO**    | **Tipo Estado: Activo**<br><br>Este estado es el resultado de un comando de ARRANQUE desde el estado DESHABILITADO, con la máquina sin alarmas activas.<br><br>En este estado la máquina hará que los dispositivos se energicen y coloquen la máquina en el estado HABILITADO donde esperará la petición de ARRANQUE.          | Esta actividad se realiza al recibir una petición de ARRANQUE desde el estado DESHABILITADO.                                                                            |
| **PARANDO**        | **Tipo Estado: Activo**<br><br>A este estado se ingresa en respuesta a un comando de PARADA. Mientras está en este estado, la máquina ejecuta la lógica que la lleva a una parada controlada, para llegar al estado HABILITADO.                                                                                                | Como resultado de una petición de paro, tanto por parte del operario como consecuencia de una alarma con paro controlado. La máquina finalizará en el estado HABILITADO |
| **DESHABILITANDO** | **Tipo Estado: Activo**<br><br>El estado DESHABILITANDO se puede ingresar en cualquier momento en respuesta al comando DESHABILITAR o ante la ocurrencia de una falla en la máquina. La lógica de DESHABILTIAR hará que la máquina se detenga rápidamente de forma segura.                                                     | Como resultado de CUALQUIER paro critico de la máquina.                                                                                                                 |
| **DESHABILITADO**  | **Tipo Estado: Espera**<br><br>La máquina mantiene información de estado relevante para la condición DESHABILITADO. La máquina solo puede salir del estado DESHABILITADO después de un comando de ARRANQUE explícito, posteriormente a la intervención manual para corregir y restablecer las fallas detectadas en la máquina. | Como resultado de CUALQUIER paro crítico. La máquina permanecerá en el estado DESHABILITADO hasta que se resuelva la falla y se realice una solicitud de ARRANQUE.      |

## 2.3. Módulos y estaciones

La secuencias que controlan los módulos de la máquina (ciclos) y las estaciones contienen los mismos estados de secuencia. El siguiente diagrama muestra los diferentes estados y transiciones de la secuencia:

Diagrama de estados

Como se puede observar el diagrama es idéntico al diagrama de operación de máquina, la única diferencia son los estados que comparten el módulo (ciclo) con las estaciones una vez la secuencia esta en el estado MARCHA.

La siguiente tabla enumera los diez estados del modelo y la implementación detallada para aclarar la implementación con la intervención del Operador según corresponda para clarificarlo.

En los ciclos de cada módulo de la máquina, así como en las estaciones.

| Estado                | **Descripción**                                                                                                                                                                                                                                                                                                                | **Implementación detallada**                                                                                                                                            |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **HABILITADO**        | **Tipo Estado: Espera**<br><br>La máquina está encendida y posicionada después de completar el estado de HABILITACIÓN O DE PARADA. Todas las comunicaciones con otros sistemas están funcionando (si corresponde).                                                                                                             | La máquina ha sido “re-posicionada” correctamente  y está esperando la solicitud de ARRANQUE.                                                                           |
| **INICIANDO**         | **Tipo Estado: Activo**<br><br>La máquina completa los pasos necesarios para comenzar. Este estado se ingresa como resultado de un comando de ARRANQUE (local). Siguiendo este comando, la máquina comenzará a ponerse en marcha.                                                                                              | Petición de ARRANQUE desde el estado ENABLED.                                                                                                                           |
| **EN MARCHA**         | **Tipo Estado: Activo**<br><br>Una vez que la máquina está procesando materiales, se encuentra en el estado RUNNING.                                                                                                                                                                                                           | Ejecución por una solicitud de ARRANQUE desde el estado HABILITADO. La máquina está produciendo producto a la velocidad de producción deseada                           |
| **HABILITANDO**       | **Tipo Estado: Activo**<br><br>Este estado es el resultado de un comando de ARRANQUE desde el estado DESHABILITADO, con la máquina sin alarmas activas.<br><br>En este estado la máquina hará que los dispositivos se energicen y coloquen la máquina en el estado HABILITADO donde esperará la petición de ARRANQUE.          | Esta actividad se realiza al recibir una petición de ARRANQUE desde el estado DESHABILITADO.                                                                            |
| **PARANDO**           | **Tipo Estado: Activo**<br><br>A este estado se ingresa en respuesta a un comando de PARADA. Mientras está en este estado, la máquina ejecuta la lógica que la lleva a una parada controlada, para llegar al estado HABILITADO.                                                                                                | Como resultado de una petición de paro, tanto por parte del operario como consecuencia de una alarma con paro controlado. La máquina finalizará en el estado HABILITADO |
| **DESHABILITANDO**    | **Tipo Estado: Activo**<br><br>El estado DESHABILITANDO se puede ingresar en cualquier momento en respuesta al comando DESHABILITAR o ante la ocurrencia de una falla en la máquina. La lógica de DESHABILTIAR hará que la máquina se detenga rápidamente de forma segura.                                                     | Como resultado de CUALQUIER paro critico de la máquina.                                                                                                                 |
| **DESHABILITADO**     | **Tipo Estado: Espera**<br><br>La máquina mantiene información de estado relevante para la condición DESHABILITADO. La máquina solo puede salir del estado DESHABILITADO después de un comando de ARRANQUE explícito, posteriormente a la intervención manual para corregir y restablecer las fallas detectadas en la máquina. | Como resultado de CUALQUIER paro crítico. La máquina permanecerá en el estado DESHABILITADO hasta que se resuelva la falla y se realice una solicitud de ARRANQUE.      |
| **TRABAJO A INICIAR** | **Tipo Estado: Espera**<br><br>Estado utilizado solo en las estaciones.<br><br>La estación entra en este estado cuando la estación está habilitada. Quedándose a la espera de recibir la orden o petición de TRABAJO por parte del módulo (ciclo)                                                                              |                                                                                                                                                                         |
| **TRABAJANDO**        | **Tipo Estado: Activo**<br><br>Estado utilizado solo en las estaciones.<br><br>La estación ha recibido la petición de TRABAJO y realiza la operación.                                                                                                                                                                          |                                                                                                                                                                         |
| **TRABAJO REALIZADO** | **Tipo Estado: Espera**<br><br>Estado utilizado solo en las estaciones.<br><br>La estación ha finalizado la operación y espera el reconocimiento de trabajo finalizado por parte del módulo (ciclo) para pasar de nuevo al estado TRABAJO A INICIAR si la estación está habilitada.                                            |                                                                                                                                                                         |

## 3. Estructuración del programa

El programa se divide en cinco partes o programas generales de control, los programas específicos de control de módulos (ciclos) con sus estaciones y el programa de seguridad.

Cada programa contiene diferentes rutinas para la gestión de los diferentes elementos de control específicos.

A continuación se detallan las diferentes partes del programa.

## 3.1. Control Seguridad

El control de las señales de seguridad de la máquina se controlan en un programa específico, dentro de la tarea de seguridad “SafetyTask”.

Dentro del control de seguridad de máquina se gestionan las diferentes áreas y los diferentes elementos de seguridad.

En la siguiente captura de pantalla podemos ver la distribución de las diferentes áreas y elementos:

## 3.1. 1. Rutinas control seguridad

**R00_Initialization – Inicialización de variables:**

En el caso que nos ocupa, la termoformadora puede disponer de tres zonas de seguridad diferentes:

-       Módulo 1 – Zona Bolsas

-       Módulo 2 – Zona conectores

-       Módulo 3 – Zona Taponado bolsas

Dentro de la inicialización de seguridad se activan o desactivan los diferentes módulos, dependiendo de si la termoformadora dispone o no de esos módulos.

Eso nos permite poder habilitar o deshabilitar zonas de seguridad, dependiendo de la configuración general de la máquina.

**R01_General_Inputs:**

Gestión de los elementos de entrada general de máquina. Por ejemplo, pulsadores rearme de los paros de emergencia.

**R02_General_Outputs****:**

Gestión de los elementos de seguridad de salida generales. Generalmente se aplica a los relés generales de máquina que dan liberación a máquinas externas.

**R03_General_EnableAxis:**

Gestión de las habilitaciones de seguridad para los servomotores.

**R04_General_Status:**

Gestión de los estados generales de seguridad. Se activan los bits generales de puertas de seguridad OK, paros de emergencia,…

En esta parte del programa se realizan las tareas generales.

**R11_Mod1xxxx_Sagfety_Inputs:**

Gestión de todos los elementos de seguridad de entrada del módulo. Puertas, setas de emergencia, barreras,…

**R12_Mod1xxxx_Sagfety_Outputs:**

Gestión de todos los elementos de seguridad de salida del módulo. Relés de seguridad, habilitaciones,…

**R13_Mod1xxxx_Sagfety_Axis:**

Gestión de la activación y desactivación de la seguridad de los servomotores.

## 3.2. Control Hardware

El control de los elementos de hardware de la máquina se realizan en el programa **“P00_Hardware”**.

En esta parte del programa se gestiona todo lo relativo al control de hardware de la máquina.

En la siguiente captura de pantalla podemos ver las rutinas del programa de control de hardware:

## 3.2.1. Rutinas control Hardware

**R02_Ethernet_IP:**

Lectura del estado de las comunicaciones con los equipos ethernetIP de la máquina.

**R07_NTP_Clock_Sync:**

Control de la sincronización horaria.

## 3.3.  Control General

Los elementos y partes de programa que son genéricos, se gestionan dentro del programa **“P001_MachineGeneral”**.

En la siguiente captura de pantalla podemos ver las rutinas del programa de control general:

## 3.3.1. Rutinas control general

**R01_Safety:**

Envio del reset de máquina al programa de seguridad.

**R03_StationPosCtrl:**

Control y limitación de las posiciones de las estaciones del módulo 1.

**R05a_HMI_MessageList:**

Configuración del listado de mensajes para el HMI.

**R05a_HMI_PB:**

Gestión de botones del HMI.

**R16_Timers:**

Generación de intermitencias con diferentes periodos.

**R17_Light:**

Gestión de las luces de la máquina. Balizas, botones del panels de control,…

**R18_24VccFusesCtrl:**

Control de las protecciones eléctricas de la máquina.

**R20_CompressAir:**

Control del aire general de la máquina.

## 3.4. Control Estadísticas

El control de las estadísticas de la máquina (contadores, producción,…) se generan dentro del programa **“P010_MachineStatistics”**.

Dentro de este programa encontraremos las rutinas de gestión de:

o    Contadores producción

o    Control registro

o    Tiempos ciclo

## 3.4.1. Rutinas control estadísticas

**R02_Counters:**

Control de los contadores de producción de máquina.

Contadores:

o    Total producido.

o    Total producción correcta.

o    Total producción rechazada.

o    Total rechazos por estación.

o    Total rechazos por posición elemento.

**R02_ShiftRegister:**

Gestión reset del registro, generación estados máquina vacia,…

**R03_Production:**

Gestión tiempos funcionamiento máquina, ciclo máquina,…

**R04_CycleTimesControl:**

Control del tiempo máximo de ciclo de la máquina.

## 3.5. Control Alarmas

Las alarmas de la máquina se generan y se gestionan en el programa **“P015_MachineAlarms”**.

Las rutinas dentro de este programa se ocupan de:

o    Activar las alarmas según Generar las alarmas según las condiciones.

o    Gestionar el reconocimiento de las alarmas activas.

o    Gestionar el reset de las alarmas activas y reconocidas.

o    Generar sumario de alarmas.

## 3.5.1. Rutinas control alarmas

**R01a_SetFaultsGeneral:**

Generación alarmas generales de máquina, tales como:

- Elementos de seguridad (paros emergencia, puertas,…)
- Protecciones eléctricas máquina
- Contactores alimentación servos
- Sensores referencia robot

Las alarmas se diferencian en dos tipos:
- Paro en posición (STOP)
- Paro inmediato (ABORT).

Para realizar esta diferenciación hay que indicar de que tipo es la alarma activada.

Por defecto todas las alarmas son del tipo STOP, si se indica lo contrario esa alarma pasa a ser de tipo ABORT.

Esta diferenciación se utiliza en la rutina “R05a_Summary” para crear el bit resumen de fallo activo tipo STOP o fallo activo tipo ABORT.

Dentro de la estructura de las alarmas disponemos de temporizadores para filtrar las señales que generan las alarmas.

**R01b_SetFaultsCommunications:**

Generación alarmas comunicaciones con elementos ethernet/IP.

**R01c_SetFaultsBagZone:**

Generación alarmas del módulo de bolsas, módulo 1.

**R02_AckFaults:**

Gestión del reconocimiento de alarmas. Las alarmas activas se reconocen al recibir la petición desde el HMI.

**R03_ResetFaults:**

Gestión del reset de las alarmas activas y reconocidas. Solo las alarmas reconocidas pueden ser reseteadas.

**R04_SetWarnings:**

Generación de los avisos previos a la alarma.

**R05_Summary:**

Generación de los bits resumen de alarmas. Se revisan todos los bits de alarma para activar un solo bit resumen que indica que alguna alarma esta activa.

## 3.6.  Control Máquina

En ``MachineControl`` están las rutinas de control principales de máquina.

Control del lote y control de la operación general.

## 3.6.1.        Rutinas control proceso de lote (Batch process)

**R02a_BatchProcess_HMI_PBControl:**

Control de las peticiones recibidas desde HMI durante el proceso de inicio de lote. Se generan los mensajes de aviso al operario.

**R02b_BatchProcessWizard:**

Control del wizard de inicio de lote. La rutina se encarga de gestionar y controlar los pasos del wizar de inicio de lote.

**R02c_BatchProcessCommands:**

Control de las peticiones de control del secuenciador. Una vez finalizado el wizar de inicio de lote, se reciben los comandos desde HMI para activar, desactivar o abortar el lote.

**R02d_BatchProcessManagement:**

Control del secuenciador del proceso del lote.

Es la parte encarga de realizar las transiciones del secuenciador entre los pasos de espera y los pasos activos.

**R02f_BatchProcessSequence:**

Secuenciador proceso lote.

Se encarga de gestionar las diferentes pasos de secuencia, realizando las operaciones necesarias en cada caso.

**R02g_BatchProcess_Recipes:**

Gestión parámetros receta y parámetros máquina.

## 3.6.2. Rutinas control operación máquina (Operation)

**R03a_OperationModes:**

Control de los modos de operación de máquina.

**R03b_OperationCommands:**

Control de las peticiones externas. Pulsadores marcha, paro,…

**R03c_OperationManagement:**

Control del secuenciador de operación de máquina.

Es la parte encarga de realizar las transiciones del secuenciador entre los pasos de espera y los pasos activos.

**R03d_OperationSequence:**

Secuenciador operación máquina.

Se encarga del control del secuenciador principal de la máquina. Este secuenciador es el encargado de enviar las órdenes a los módulos (ciclos) de la máquina y esperar que estos dan la confirmación de que han finalizado la transición ordenada.

## 3.7. Control Módulos

El programa que controla el módulo (ciclo), recibe los comandos activados por el control de operación de máquina. El secuenciador del módulo controla que estaciones deben funcionar enviándoles las ordenes de trabajo y esperando que estas finalicen su función.

**R05_Cycle_Management:**

Control del secuenciador de ciclo del módulo.

Es la parte encarga de realizar las transiciones del secuenciador entre los pasos de espera y los pasos activos.

**R06_Cycle_Sequence:**

Secuenciador ciclo del módulo máquina.

Se encarga del control del secuenciador del módulo (Ciclo). Este secuenciador es el encargado de decidir que estaciones deben iniciar su función y espera que las estaciones hayan finalizado dicha función.

## 3.8. Control Estaciones

Es el programa que controla el módulo (ciclo). Recibe los comandos activados por el control de operación de máquina.

El secuenciador del módulo controla que estaciones deben funcionar enviándoles las ordenes de trabajo y esperando que estas finalicen su función.

**R10_Devices:**

Control de los dispositivos de la estación, tales como:

o    Válvulas

o    Motores AC

o    Servo motores

o    cámaras

o    …

**R20_Application_Management:**

Control del secuenciador de la estación.

Es la parte encarga de realizar las transiciones del secuenciador entre los pasos de espera y los pasos activos.

**R21_Application_Sequence:**

Secuenciador de la estación.

Se encarga del control del secuenciador de la estación. El secuenciador realizará las operaciones necesarias para la funcionalidad esperada.

## 4. Nomenclatura variables

===ya lo he movido a su sitio

## 4.1. Prefijos



o    Entradas digitales -> diXxxx
o    Salidas digitales -> doXxxx
o    Entradas analógicas ->aiXxxx
o    Salidas analógicas -> aoXxxx
o    Entradas digitales seguras -> siXxxx
o    Salidas digitales seguras -> soXxxx
o    Estados -> stsXxxx
o    Peticiones/Comandos -> cmdXxxx
o    Servo motores -> axYxxx
o    Auxiliares ( de uso local en las rutinas) -> auxYxxx

## 4.2. Variables Genericas

Para una mejor interpretación de las variables, se utilizan unos prefijos para diferentes tipos de variables, dependiendo del uso de la misma.

o    alwaysFalse -> Bit siempre a 0
o    alwaysTrue -> Bit siempre a 1
o    stationOFF -> Bit indicación estación apagada.
o    taskPending -> Bit indicación tarea pendiente

## 4.3. Nombres Estructuras Datos

o    seqCtrl -> Estructura datos control secuencia
o    command -> Estructura datos peticiones
o    status -> Estructura datos estados
o    devices -> Estructura datos dispositivos
o    param -> Estructura datos parámetros
o    values -> Estructura datos valores
o    timers -> Estructura datos temporizadores
o    safety -> Estructura datos seguridad
o    batchProcess -> Estructura datos proceso lote
o    operation -> Estructura datos control operación máquina
o    HMI -> Estructura datos visualización HMI
o    doors -> Estructura datos control puertas
o    alarms -> Estructura datos alarmas
o    counters -> Estructura datos contadores producción
o    lights -> Estructura datos luces i aviso acustico máquina
o    compressAir -> Estructura datos control aire comprimido máquina
o    stationsPos -> Estructura datos posición estaciones con referencia al registro desplazamiento.
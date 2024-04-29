---
draft: false
---
Para agilizar la interpretación del programa, se definen unos prefijos en las variables que ayudan a identificarlas y reconocer de que tipo son. A continuación se detallan los diferentes campos donde se aplica.

# 01. Entradas y Salidas

Las entradas y salidas deben declararse en _ControllerTags_ y se identificaran con un prefijo de dos letras en función del tipo: digital, analógica o seguras:

> [!todo] Input/Output
> di  =  Digital Input  
> do = Digital Output  
> ai  = Analog Input  
> ao = Analog Output  
> si  = Safety Input  
> so = Safety Output  

Seguido del prefijo hay que indicar el módulo ``ModX`` al que pertenece. A continuación se debe acompañar una barra baja y el nombre de la entrada/salida a título descriptivo.

> _prefijo + ModX + _ + IOName_

En el nombre de la variable **NO** hay que escribir el código eléctrico identificador del sensor; ese código debe ser introducido en el campo _Description_, para que quede constancia de la relación y además pueda ser filtrado en las búsquedas.

>[!example]+ Ejemplo
>![[Pasted image 20240426173256.png]]


# 02. Ejes

Los ejes deben llamarse con el prefijo ``ax`` y el módulo al que corresponden ``ModX`` seguidamente de una barra baja y la correspondiente referencia a la estación donde se usa y al nombre del propio eje.

> ***ax + ModX + _ + Station + ServoName

> [!example]+ Ejemplo
> axMod1_FillingPump1  
> axMod2_TransferStarwheel  

El _MotionGroup_ debe llamarse siempre igual en todas las máquinas:
==XXXX



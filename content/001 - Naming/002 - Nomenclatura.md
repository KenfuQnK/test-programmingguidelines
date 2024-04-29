---
draft: false
---

# 01. Dispositivos del Hardware

Los dispositivos del hardware en la lista de _Devices Explorer_ deben ser nombrados con el código eléctrico identificativo.


# 02. Variables

Para una mejor interpretación del significado de las variables utilizadas en el programa, se definen unas normas de uso. Estas normas deben facilitar la interpretación del programa entre los programadores.

Las variables deben empezar en **Mayúscula** si se trata de una palabra significativa, y en **minúscula** si se trata de una palabra o abreviatura que define la naturaleza de la variable. Si es necesario crear una variable con más de una palabra, el cambio de palabra se hará con la inicial en **mayúscula**.

___
A continuación se muestran varios ejemplos:

- Cada palabra significativa debe empezar en Mayúscula

> [!success]+ Correcto
> mod1XXX_Machine.Command.Reset  
> mod1XXX_Machine.Command.Home  
> mod1XXX_stLoading.Status.InPick  
> mod1XXX_stLoading.Status.PartsPicked  

> [!failure]+ Incorrecto
> mod1XXX_stLoading.Status.partsPicked  
> mod1XXX_StLoading.status.Partspicked  


- Después de cualquier palabra anterior, la siguiente si viene unida debe ser también en Mayúscula

> [!success]+ Correcto
> aiMod1_AirPressure  

> [!failure]+ Incorrecto
> aimod1_Airpressure  
> aiMod1_airPressure  


- Prefijos auxiliares o abreviaciones que indiquen la naturaleza del tag deben empezar en minúscula:

> [!success]+ Correcto
> mod1_Machine  
> auxActivateSomething  
> stFilling  
> strText  

> [!failure]+ Incorrecto
> StrText  


- Las barrabajas deben usarse cuando se desee agrupar, y no deben usarse simplemente para separar palabras:

> [!success]+ Correcto
> diMod1_RejectCylinderReed_Home  
> diMod1_RejectCylinderReed_Work  
> diMod1_GripperCylinderReed_Home  
> diMod1_GripperCylinderReed_Work  

> [!failure]+ Incorrecto
> stReject.Status.Vial_has_to_be_reject  


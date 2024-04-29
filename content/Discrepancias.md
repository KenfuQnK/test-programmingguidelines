---
draft: true
---


# 1.1. Activar/desactivar estaciones

Se permite activar/desactivar estaciones en cualquier estado que no sea en movimiento (running, stopping, etc).

# 1.2. Inhibir e Ignorar

Diferenciar inhibir de Ignorar. Ignorar es que la estación no hace nada, no funciona, esta rota, etc. Y por tanto no se energizará al pasar a activa. No saldrán alarmas relacionadas. 
Inhibir es cuando sí existe para la máquina, pero no trabajará.

# 1.2. Lógica negativa

Siempre lógica negativa. El producto es defectuoso siempre mientras no se terminen todas las tareas. Con la suma de todas las acciones y sus correspondientes controles de calidad, en caso de que todo sea correcto entonces y sólo entonces se considerará el producto como correcto. En todos los otros casos, carril de rechazo. También debe rechazarse una posición vacía, para evitar cualquier riesgo.

Considerar que si una estación ha sido deshabilitada o se ha deshabilitado el rechazo por ese motivo, dicho producto será considerado como bueno (siempre y cuando lo único que no haya cumplido haya sido eso). Esa deshabilitación irá con tras firma electrónica.

Adicionalmente, debe trabajarse con el departamento de diseño mecánico la lógica negativa a nivel mecánico. Un ejemplo de diseño correcto (en cuanto a lógica negativa) es la dosificadora de hematíes GE208002.

En este caso se trata de una máquina de baja cadencia e intermitente, pero es para entender el concepto de lógica negativa a nivel mecánico. No siempre es posible, pero debe SIEMPRE evaluar la mejor solución mecánica y de software sobre este aspecto. Producto incorrecto por el carril de correctos es inaceptable y menos en la industria farmacéutica.
Cristian Yahir Campos Aparicio. SMSS109222
Lindys Arely Martinez Herrera. SMSS170822
Sistema web para control de mantenimiento y facturación de equipos informáticos

Situación problemática

En talleres de reparación de computadoras, laboratorios de informática o pequeños negocios de soporte técnico, muchas veces no existe un control ordenado de los equipos que ingresan a mantenimiento. Esto genera problemas como pérdida de información del cliente, desorden en el seguimiento del estado de reparación, dificultad para calcular costos del servicio y falta de comprobantes claros al momento de entregar el equipo.

Sector al que va enfocado
La solución está dirigida a:
- Talleres de reparación de computadoras
- Negocios de soporte técnico
- Laboratorios de informática
- Centros escolares o universitarios con área técnica

Solución propuesta
Se desarrolló una aplicación web en Vue.js que permite registrar equipos que ingresan a mantenimiento, guardar la información del cliente, describir la falla reportada, actualizar el estado del proceso de reparación, registrar costos del servicio y generar una factura cuando el equipo ya ha sido reparado o entregado.

Funciones del sistema
- Registrar nuevos equipos para mantenimiento
- Mostrar una lista de equipos registrados
- Actualizar el estado de cada reparación
- Calcular automáticamente el costo total del servicio
- Generar una factura o comprobante de servicio
- Validar que los campos obligatorios no estén vacíos
- Eliminar registros incorrectos
- Guardar la información en localStorage para no perder los registros al recargar la página

Preguntas:

1. Explique con sus propias palabras qué es Vue.js y cuál es su función dentro de la página web desarrollada:

Vue.js es un framework de JavaScript que sirve para crear interfaces web dinámicas e interactivas. En este proyecto, Vue.js permite enlazar los datos del formulario con la interfaz, actualizar automáticamente la lista de equipos, mostrar mensajes, calcular valores y generar la factura sin necesidad de recargar la página.

2. Describa qué variables reactivas utilizó en su aplicación y cuál es la función de cada una dentro del sistema:

Las variables reactivas utilizadas fueron:

- `cliente`: almacena el nombre del cliente.
- `tipoEquipo`: guarda el tipo de equipo que ingresó al sistema.
- `marca`: almacena la marca del equipo.
- `modelo`: guarda el modelo del equipo.
- `falla`: describe el problema reportado.
- `fechaIngreso`: almacena la fecha en la que el equipo fue recibido.
- `estado`: guarda el estado actual del mantenimiento.
- `costoRevision`: registra el costo de la revisión técnica.
- `costoReparacion`: registra el costo de la reparación realizada.
- `costoRepuestos`: registra el costo de los repuestos utilizados.
- `equipos`: almacena todos los registros ingresados en una lista.
- `facturaSeleccionada`: guarda el equipo al que se le mostrará la factura.
- `mensajeError`: muestra mensajes de error de validación.
- `mensajeExito`: muestra mensajes de éxito al registrar datos.

3. Explique la diferencia entre las siguientes directivas utilizadas en su proyecto: v-bind y v-model.
La directiva `v-bind` se utiliza para enlazar atributos de un elemento HTML con datos dinámicos de Vue. En este proyecto se utiliza para asignar una clase dinámica al estado del equipo.
La directiva `v-model` se utiliza para enlazar bidireccionalmente los datos de un input con una variable reactiva. Esto significa que cuando el usuario escribe en un campo, la variable cambia automáticamente.

4. Mencione al menos un ejemplo de evento utilizado dentro de su aplicación.
Un ejemplo de evento utilizado es `@click`, que se usa en los botones para registrar un equipo, limpiar el formulario, eliminar registros, mostrar la factura e imprimirla.

5. Explique para qué utilizó la directiva v-for dentro de su aplicación.
La directiva `v-for` se utilizó para recorrer la lista de equipos registrados y mostrarlos dentro de una tabla. Gracias a esto, cada equipo agregado aparece automáticamente en la interfaz.

6. Describa en qué situación utilizó v-if y qué problema resuelve dentro de su interfaz.
La directiva `v-if` se utilizó para mostrar mensajes de error, mensajes de éxito, la tabla solo cuando existan registros y la factura únicamente cuando se haya seleccionado un equipo. Esto ayuda a que la interfaz sea más clara y evita mostrar información innecesaria.

7. Explique cómo se realiza la validación de datos en su aplicación y por qué es importante validar la información ingresada por el usuario.
La validación se realiza comprobando que los campos obligatorios no estén vacíos y que los costos no tengan valores negativos. Esto es importante porque evita guardar información incompleta o incorrecta, mejora el funcionamiento del sistema y reduce errores durante el proceso de mantenimiento y facturación.

Uso de localStorage
En esta aplicación se utiliza `localStorage` para guardar los registros en el navegador del usuario. Esto permite que, aunque la página se recargue o se cierre temporalmente, los datos permanezcan disponibles. Sin embargo, esta información se guarda solo de manera local en el navegador y no en una base de datos en línea.

Tecnologías utilizadas
- Vue.js
- JavaScript
- HTML
- CSS
- Vite

Ejecución del proyecto

```bash
npm install
npm run dev# mantenimiento-equipos





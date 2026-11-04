# Focus

Focus es una aplicación web para planificar el día de trabajo y medir el tiempo real que le dedicas a cada tarea. Le asignas un tiempo a cada tarea, trabajas con una cuenta regresiva a la vista y, al final, puedes comparar lo planificado con lo que realmente hiciste.

Toda la aplicación está en un solo archivo, [`focus.html`](focus.html). No necesita instalación, servidor ni cuenta de usuario, y tus datos se guardan únicamente en tu navegador.

## Cómo empezar

1. Abre `focus.html` en un navegador moderno.
2. Agrega una tarea con su tiempo planificado, por ejemplo `Informe mensual` y `1h 30m`.
3. Pulsa **Iniciar** y trabaja. Focus te avisa cuando se acerca el final y cuando se acaba el tiempo.

## Las tres vistas

### Hoy

Es la pantalla principal. Muestra la tarea actual con su cuenta regresiva, el tiempo transcurrido y una barra de progreso, junto al plan del día.

Puedes armar el plan de tres formas:

- **Formulario:** nombre, tiempo y, si quieres, una nota y una categoría. Mientras escribes, sugiere tareas que ya existen y advierte si el nombre se parece a otra, para evitar duplicados.
- **Pegar una lista:** convierte un texto en tareas y muestra una vista previa antes de agregarlas.
- **Copiar de mi último día:** reutiliza las tareas de tu último plan, con la opción de usar solo el tiempo que faltó.

### Calendario

Muestra por mes el tiempo trabajado cada día. Al abrir un día ves el detalle de sus sesiones, y puedes corregirlas, eliminarlas o agregar tiempo manualmente.

### Tareas

Es el catálogo de todas tus tareas, con buscador. Desde aquí puedes editarlas, archivarlas, restaurarlas, fusionar dos tareas en una o eliminarlas.

> Eliminar una tarea también borra sus sesiones y su presencia en los planes. Si quieres conservar el historial, archívala.

## Formato de tiempos

| Escribes | Significa |
| --- | --- |
| `45` | 45 minutos (un número solo son minutos) |
| `30m`, `30 min` | 30 minutos |
| `1h`, `2 horas` | 1 hora, 2 horas |
| `1h 30m` | 1 hora y 30 minutos |
| `1.5h`, `1,5h` | 1 hora y 30 minutos |
| `1/2 h` | 30 minutos |
| `1 1/2 h` | 1 hora y 30 minutos |

## Formato para pegar listas

Cada tarea lleva su tiempo entre paréntesis. Una línea que termina en dos puntos define una categoría, y el texto después del tiempo se guarda como nota.

```text
Estudio:

Java Spring Boot (3h): revisando ejecuciones en batch
Base de Datos (Oracle) (2h)

Proyectos en Marcha:

MyPlanner (1h)
Libro (1/2 h)
```

## Atajos de teclado

| Tecla | Acción |
| --- | --- |
| `Espacio` | Iniciar o pausar |
| `N` | Pasar a la siguiente tarea |
| `+` | Añadir 5 minutos |

## Avisos

Focus te avisa dos veces durante cada tarea: antes de terminar (por defecto, 5 minutos antes) y cuando se acaba el tiempo. Si la tarea dura menos que ese margen, el primer aviso llega a la mitad.

Los avisos combinan un mensaje en pantalla y un destello con sonido y notificaciones del sistema, que son opcionales. La cuenta sigue funcionando con la pestaña en segundo plano.

## Otras funciones

- **Mini ventana:** una ventana flotante siempre visible con el reloj y el botón de iniciar o pausar. Solo aparece en navegadores compatibles con Document Picture-in-Picture, como Chrome y Edge.
- **Reloj olvidado:** si Focus estuvo cerrado o el equipo en reposo más de 10 minutos con una tarea en marcha, te pregunta si cuenta todo ese tiempo o solo hasta el último momento en que estuvo activo.
- **Inicio de día configurable:** por defecto, el día empieza a las 4:00, así que lo que trabajes de madrugada cuenta en el día anterior. Si una tarea sigue en marcha durante el cambio de día, la sesión se divide y la tarea continúa en el día nuevo.
- **Una sola pestaña activa:** si abres Focus en otra pestaña, esta queda bloqueada para que el tiempo no se cuente dos veces.
- **Tema claro y oscuro:** sigue la preferencia de tu sistema.

## Tus datos

Focus no envía tus datos a ningún servidor. Todo se guarda en IndexedDB, dentro del navegador que uses, y funciona sin conexión.

Esto significa que los datos no se sincronizan entre navegadores o equipos, y que puedes perderlos si borras los datos de navegación. Para protegerlos, usa **Ajustes → Copia de seguridad**:

- **Exportar** descarga un archivo JSON con tus tareas, planes, sesiones y ajustes.
- **Importar** restaura una copia y **reemplaza todo lo guardado**, previa confirmación.

Focus te recuerda hacer una copia si llevas más de 30 días sin una.

> Los datos dependen del lugar desde donde abres la aplicación. Si mueves `focus.html` a otra carpeta o lo sirves desde otra dirección, es posible que no veas tus datos anteriores. Exporta una copia antes de moverlo.

## Notas

`notas.html` organiza tus apuntes en cuadernos y páginas:

- **Cuadernos:** puedes crearlos, renombrarlos y eliminarlos.
- **Reordenar:** mantén pulsado un momento un cuaderno o una página y arrástralo. El retraso evita moverlos por accidente. Con el teclado, usa `Alt` + `↑` o `↓`. Mientras haya una búsqueda activa, las páginas no se pueden mover.
- **Páginas:** tienen título, color y buscador por cuaderno. Se guardan solas mientras escribes.
- **Formato:** una barra fija en la parte superior de la página ofrece negrita, cursiva, subrayado, estilos, color, resaltado, listas y quitar formato.
- **Checklist:** el botón del pie de la página convierte cada línea del texto en un elemento por marcar, y **Texto libre** lo devuelve a líneas. Si no cambiaste los elementos, el texto vuelve idéntico, con su formato; si los cambiaste, vuelve con una línea por elemento y sin formato. Las marcas se recuerdan y las imágenes se quedan donde están.
- **Imágenes:** consulta [Imágenes en las notas](#imágenes-en-las-notas).

Sus datos están separados de los de Focus, en otra base de IndexedDB, y tienen su propia copia de seguridad con los botones **Exportar** e **Importar**. Importar **reemplaza todas las notas**, previa confirmación. La copia de Focus no incluye las notas.

### Imágenes en las notas

Pega una imagen con `Ctrl+V` o arrastra un archivo de imagen a la página. Se admiten PNG, JPEG, WebP y GIF. Las imágenes quedan **encima de la hoja**: no desplazan el texto y puedes dejarlas en cualquier punto.

- **Seleccionar:** haz clic en la imagen. En táctil, tócala.
- **Mover:** arrástrala a donde quieras. Lo arrastrado queda encima de las demás imágenes. Con el teclado, usa las flechas: 10 px, o 1 px con `Mayús`.
- **Cambiar el tamaño:** arrastra una esquina. Se mantiene la proporción y la esquina contraria se queda quieta.
- **Eliminar:** usa el botón de la imagen o `Supr`. El aviso ofrece **Deshacer**.

Las imágenes se reducen a 1600 px de ancho, se comprimen en WebP y se guardan como binario en su propio almacén de IndexedDB, separadas del texto. La copia de seguridad las incluye en base64.

Ten en cuenta estos límites:

- La imagen no sigue al texto: si escribes más arriba, el texto pasa por debajo de ella. El texto tapado no se puede seleccionar hasta que muevas la imagen.
- Su posición horizontal y su ancho son proporcionales a la hoja, y su altura se mide desde el borde superior. Con otro ancho de ventana el texto se reacomoda, así que puede quedar sobre otra parte del texto.
- `Ctrl+Z` no afecta a las imágenes.
- Al pegar desde una web texto con imágenes, solo se conserva el texto. Las imágenes externas se descartan para no cargar nada de internet sin que lo pidas.
- El buscador no encuentra el contenido de las imágenes.

## Tecnología

Focus está hecho con HTML, CSS y JavaScript puros, sin frameworks, dependencias ni proceso de compilación. Para modificarlo, edita `focus.html` y recarga la página.

Usa estas APIs del navegador:

- **IndexedDB:** almacenamiento de los datos.
- **Web Workers:** reloj estable con la pestaña en segundo plano.
- **Web Locks:** bloqueo para mantener una sola pestaña activa.
- **Web Audio y Notifications:** avisos.
- **Document Picture-in-Picture:** mini ventana.

## Créditos

Desarrollado por [BYGUEN - Custom Development](https://www.byguen.com).

© 2026 BYGUEN. Todos los derechos reservados.

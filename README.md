# Mi día

Mi día es una aplicación web para planificar el día de trabajo y medir el tiempo real que le dedicas a cada tarea. Le asignas un tiempo a cada tarea, trabajas con una cuenta regresiva a la vista y, al final, puedes comparar lo planificado con lo que realmente hiciste.

Toda la aplicación está en un solo archivo, [`midia.html`](midia.html). No necesita instalación, servidor ni cuenta de usuario, y tus datos se guardan únicamente en tu navegador.

## Aplicaciones del repositorio

Cada aplicación es un único archivo HTML autocontenido, con sus propios datos, y funciona por sí sola:

| Archivo | Qué es |
| --- | --- |
| [`index.html`](index.html) | Lanzador: un menú siempre visible con cada aplicación abierta a su lado. |
| [`midia.html`](midia.html) | Planificación del día y medición del tiempo. Es lo que describe la mayor parte de este documento. |
| [`notas.html`](notas.html) | Apuntes en cuadernos y páginas. Consulta [Notas](#notas). |
| [`navaja.html`](navaja.html) | Herramientas para tu día en la oficina. Consulta [Navaja suiza](#navaja-suiza). |

### Lanzador

Abre `index.html` para tener todas las aplicaciones en un mismo lugar:

- **Cambiar de aplicación no interrumpe nada:** cada una sigue viva en su propio marco, y el reloj de Mi día continúa aunque estés en otra.
- **El reloj siempre a la vista:** mientras hay una tarea en curso, el menú muestra su nombre y su tiempo; sin tarea en curso, muestra la descripción de Mi día. El título de la pestaña también muestra la tarea y el tiempo.
- **Menú contraíble:** el botón de la parte superior lo deja solo con íconos. En móvil pasa a ser una barra inferior.
- **Abrir aparte:** lleva la aplicación a una pestaña propia, y **Traer aquí** la devuelve.

- **Mini ventana:** funciona también dentro del lanzador y sigue activa aunque cambies de aplicación. Al volver desde ella a la pestaña, el lanzador muestra Mi día con la tarea en marcha.

Para agregar una aplicación, añade una línea a la lista `APPS` al inicio del script de `index.html`, y bórrala para quitarla. Una aplicación funciona igual sin el lanzador, y se comunica con él por `postMessage`, como hace Mi día en su sección "Lanzador":

| La aplicación envía | Para qué |
| --- | --- |
| `title` | Mostrar su estado en el menú y en la pestaña. |
| `pip-open`, `pip-close` | Abrir o cerrar la mini ventana. Solo se abre como respuesta a un clic del usuario. |
| `pip-paint` | Actualizar la mini ventana: `title`, `value`, `foot`, `action`, `actionDisabled` y `state`. |
| `detach` | Pasar la aplicación a una pestaña propia. |
| `download` | Guardar un archivo: `id`, `name` y `blob`. Lo hace el lanzador, porque el navegador bloquea «Guardar como» y las descargas que inicia un marco de otro origen. |

| La aplicación recibe | Cuándo |
| --- | --- |
| `pip-opened`, `pip-closed`, `pip-failed` | La mini ventana se abrió, se cerró o no se pudo abrir. |
| `pip-action` | Pulsaron el botón de la mini ventana. |
| `pip-restored` | Se volvió desde la mini ventana a la pestaña: la aplicación ya está a la vista y puede mostrar lo que corresponda. |
| `download-ack`, `download-result` | El lanzador recibió el archivo (`id`), y luego cómo terminó (`status`): `saved` (se guardó con «Guardar como»), `cancelled`, `started` (descarga normal enviada) o `repeated` (ignorado por repetido). |

## Cómo empezar

1. Abre `midia.html` en un navegador moderno.
2. Agrega una tarea con su tiempo planificado, por ejemplo `Informe mensual` y `1h 30m`.
3. Pulsa **Iniciar** y trabaja. Mi día te avisa cuando se acerca el final y cuando se acaba el tiempo.

## Las tres vistas

### Hoy

Es la pantalla principal. Muestra la tarea actual con su cuenta regresiva, el tiempo transcurrido y una barra de progreso, junto al plan del día.

Puedes armar el plan de tres formas:

- **Formulario:** nombre, tiempo y, si quieres, una nota y una categoría. Mientras escribes, sugiere tareas que ya existen y advierte si el nombre se parece a otra, para evitar duplicados.
- **Pegar una lista:** convierte un texto en tareas y muestra una vista previa antes de agregarlas.
- **Copiar de mi último día:** reutiliza las tareas de tu último plan, con la opción de usar solo el tiempo que faltó.

### Calendario

Muestra por mes el tiempo trabajado cada día. Al abrir un día ves el avance de cada tarea frente a lo planificado y el detalle de sus sesiones. En el día actual puedes corregirlas, eliminarlas o agregar tiempo manualmente; los días anteriores son solo de consulta.

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

Mi día te avisa dos veces durante cada tarea: antes de terminar (por defecto, 5 minutos antes) y cuando se acaba el tiempo. Si la tarea dura menos que ese margen, el primer aviso llega a la mitad.

Los avisos combinan un mensaje en pantalla y un destello con sonido y notificaciones del sistema, que son opcionales. La cuenta sigue funcionando con la pestaña en segundo plano.

## Otras funciones

- **Mini ventana:** una ventana flotante siempre visible con el reloj y el botón de iniciar o pausar. Solo aparece en navegadores compatibles con Document Picture-in-Picture, como Chrome y Edge.
- **Reloj olvidado:** si Mi día estuvo cerrado o el equipo en reposo más de 10 minutos con una tarea en marcha, te pregunta si cuenta todo ese tiempo o solo hasta el último momento en que estuvo activo.
- **Inicio de día configurable:** por defecto, el día empieza a las 4:00, así que lo que trabajes de madrugada cuenta en el día anterior. Si una tarea sigue en marcha durante el cambio de día, la sesión se divide y la tarea continúa en el día nuevo.
- **Una sola pestaña activa:** si abres Mi día en otra pestaña, esta queda bloqueada para que el tiempo no se cuente dos veces.
- **Tema claro y oscuro:** sigue la preferencia de tu sistema.

## Tus datos

Mi día no envía tus datos a ningún servidor. Todo se guarda en IndexedDB, dentro del navegador que uses, y funciona sin conexión.

Esto significa que los datos no se sincronizan entre navegadores o equipos, y que puedes perderlos si borras los datos de navegación. Para protegerlos, usa **Ajustes → Copia de seguridad**:

- **Exportar** descarga un archivo JSON con tus tareas, planes, sesiones y ajustes.
- **Importar** restaura una copia y **reemplaza todo lo guardado**, previa confirmación.

Mi día te recuerda hacer una copia si llevas más de 30 días sin una.

> Los datos dependen del lugar desde donde abres la aplicación. Si mueves `midia.html` a otra carpeta o lo sirves desde otra dirección, es posible que no veas tus datos anteriores. Exporta una copia antes de moverlo.

## Notas

`notas.html` organiza tus apuntes en cuadernos y páginas:

- **Cuadernos:** puedes crearlos, renombrarlos y eliminarlos.
- **Antes de eliminar:** al eliminar un cuaderno o una página aparece una ventana de confirmación. Te dice qué se borra con él (sus páginas o sus imágenes) y cuántas páginas lo enlazan, porque esos enlaces quedarán rotos. Cancelar es la opción marcada, y `Esc` o un clic fuera de la ventana también cancelan. Un cuaderno eliminado no se puede recuperar. Una página sí: el aviso que aparece después ofrece **Deshacer**.
- **Reordenar:** con mouse, arrastra un cuaderno o una página directamente. En táctil, mantenlo pulsado un momento y arrástralo, porque deslizar sin más desplaza la lista. Con el teclado, usa `Alt` + `↑` o `↓`. Mientras haya una búsqueda activa, las páginas no se pueden mover.
- **Páginas:** tienen título, color y buscador por cuaderno. Se guardan solas mientras escribes.
- **Formato:** una barra fija en la parte superior de la página ofrece negrita, cursiva, subrayado, estilos, color, resaltado, listas, tablas y quitar formato.
- **Tablas:** consulta [Tablas](#tablas).
- **Checklist:** el botón del pie de la página convierte cada línea del texto en un elemento por marcar, y **Texto libre** lo devuelve a líneas. Si no cambiaste los elementos, el texto vuelve idéntico, con su formato; si los cambiaste, vuelve con una línea por elemento y sin formato. Las marcas se recuerdan y las imágenes se quedan donde están.
- **Imágenes:** consulta [Imágenes en las notas](#imágenes-en-las-notas).
- **Enlaces entre páginas:** consulta [Enlaces entre páginas](#enlaces-entre-páginas).

Sus datos están separados de los de Mi día, en otra base de IndexedDB, y tienen su propia copia de seguridad con los botones **Exportar** e **Importar**. Importar **reemplaza todas las notas**, previa confirmación. La copia de Mi día no incluye las notas.

### Tablas

Pulsa el botón de la tabla en la barra y elige el tamaño en la rejilla, hasta 10 columnas por 8 filas. Con el teclado, recorre la rejilla con las flechas y pulsa `Enter`. La tabla se inserta donde está el cursor, ocupa el ancho de la hoja con columnas iguales (luego puedes cambiarlo) y, si no hay nada debajo, deja una línea vacía para seguir escribiendo.

- **Escribir:** `Tab` pasa a la celda siguiente y `Mayús` + `Tab` a la anterior. `Tab` en la última celda agrega una fila. `Enter` hace un salto de línea dentro de la celda.
- **Filas y columnas:** las opciones de la tabla están en tres sitios. Con clic derecho en una celda aparecen todas. Con el cursor en una tabla salen dos asas: la de la izquierda abre las opciones de esa fila y la de arriba las de esa columna. El botón de la tabla en la barra trae todas, y con el cursor en una tabla aparece activo. Desde ahí puedes insertar, subir o bajar filas, insertar o mover columnas, y eliminar la fila, la columna o la tabla. Con clic derecho y `Mayús`, o con texto seleccionado, sale el menú normal del navegador.
- **Atajos:** `Ctrl+Enter` agrega una fila debajo. `Ctrl+Mayús+Retroceso` elimina la fila del cursor.
- **Ancho:** arrastra el borde entre dos columnas para repartir el ancho entre ellas. Arrastra el borde derecho para hacer más angosta toda la tabla, desde el 15 % hasta el ancho completo de la hoja; mientras arrastras ves el porcentaje. `Esc` cancela el arrastre. Doble clic en un borde interior vuelve a dejar las columnas iguales, y doble clic en el borde derecho devuelve el ancho completo. El menú también trae **Igualar ancho de columnas** y **Ancho completo**. Al insertar o eliminar una columna, las demás se reparten el espacio.
- **Ordenar y alinear:** desde el menú de una columna puedes ordenar las filas de A a Z o de Z a A. El encabezado se queda arriba y las celdas vacías van al final. Si la columna solo tiene cifras, se ordena como números y entiende formatos como `1.200,50`. **Alinear** pone la columna a la izquierda, al centro o a la derecha.
- **Fila de encabezado:** la opción del menú resalta la primera fila. Al desactivarla, todas las celdas vuelven a ser normales.
- **Deshacer:** `Ctrl+Z` deshace la tabla insertada y cada cambio de filas, columnas, encabezado, ancho, orden o alineación. Al eliminar la tabla, el aviso también ofrece **Deshacer**.
- **Líneas alrededor de la tabla:** si la tabla está al principio de la página, `Enter` al inicio de la primera celda abre una línea encima. `Retroceso` al inicio de la línea que sigue a una tabla lleva el cursor a la última celda sin quitarle el formato a esa línea. Si la línea está vacía y no es la última de la página, además la borra.
- **Pegar:** una tabla copiada de Excel, Word, Google Docs o una web se pega como tabla, sin sus estilos. Las celdas combinadas se separan y su contenido queda en la primera. Una tabla pegada dentro de una celda se reduce a texto, porque no hay tablas dentro de tablas.
- **Marcadores:** una celda puede llevar un marcador. La banderita aparece dentro de la celda.

Ten en cuenta:

- El ancho se cambia arrastrando con el mouse o el lápiz. En táctil usa **Igualar ancho de columnas** o **Ancho completo** del menú.
- En modo checklist, cada fila es un elemento, con sus celdas separadas por ` · `. Si cambias la lista y vuelves a **Texto libre**, la tabla se pierde igual que el formato.

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

### Enlaces entre páginas

Desde cualquier página puedes enlazar otra, aunque esté en otro cuaderno, como en Notion.

- **Con `@`:** escribe `@` y empieza a teclear el título. Aparece un buscador con las páginas de todos los cuadernos (el nombre del cuaderno sale debajo de cada título). Elige con `↑` `↓` y `Enter` o con un clic. `Esc` deja la `@` como texto normal.
- **La `@` de los correos no abre nada:** el buscador solo aparece si la `@` va al principio de la línea o después de un espacio. En `ana@correo.com` la `@` está pegada a otro carácter, así que se escribe tal cual.
- **Con el botón de la barra** (el icono de cadena): abre el mismo buscador. Si tenías texto seleccionado, ese texto se convierte en el enlace. Si no, se inserta el enlace en el cursor.
- **Título vivo:** el enlace insertado con `@`, o con el botón sin selección, muestra siempre el título actual de la página. Si la renombras, el enlace cambia solo. El texto que seleccionaste para un enlace se mantiene tal cual.
- **Ir y volver:** haz clic en el enlace para abrir la página, aunque esté en otro cuaderno. Arriba aparece un botón `← Título` para volver a la página de donde venías. El recorrido empieza de nuevo cuando eliges una página o un cuaderno desde las listas.
- **Enlazado desde:** al pie de cada página se listan las páginas que la enlazan.
- **Burbuja del enlace:** si dejas el ratón un momento sobre un enlace, aparece una burbuja que dice adónde lleva, con los botones **Abrir** y **Quitar enlace**. En el móvil, tocar el enlace muestra la burbuja en vez de abrirlo, y **Abrir** lleva a la página.
- **Quitar el enlace y dejar el texto:** pulsa **Quitar enlace** en la burbuja. El texto se queda como texto normal. En los enlaces de título vivo, el que queda es el título de la página. `Ctrl+Z` devuelve el enlace.
- **Enlazar una imagen:** haz clic en la imagen para elegirla y pulsa el botón de enlazar de la barra. Elige la página (y, si tiene, el marcador) en el mismo buscador. La imagen enlazada muestra un icono de cadena en su esquina. Al dejar el ratón sobre ella aparece la burbuja con **Abrir** y **Quitar enlace**. Con la imagen elegida, esos botones también están en su barra, que es como se hace en el móvil. Un clic en la imagen la elige para moverla o cambiarle el tamaño, no abre el enlace. Quitar el enlace de una imagen se deshace con **Deshacer** en el aviso, no con `Ctrl+Z`.
- **Borrar un enlace:** se borra entero, con su texto, con `Retroceso` o `Supr`, y `Ctrl+Z` lo devuelve.
- **Enlaces rotos:** si eliminas la página enlazada, el enlace se ve tachado y avisa al hacer clic. Si deshaces la eliminación, vuelve a funcionar.

#### Marcadores

Un marcador da nombre a un párrafo, título o elemento de lista para poder enlazarlo directamente, como los marcadores de Word.

- **Crear:** pon el cursor en el párrafo (o selecciona parte de su texto) y pulsa el botón de la banderita en la barra. Escribe un nombre, por ejemplo `Marcador 1`, y pulsa `Enter`. El párrafo marcado muestra una banderita en el margen izquierdo.
- **Un nombre por página:** dos marcadores de la misma página no pueden llamarse igual, sin distinguir mayúsculas ni tildes (`Prueba 1` y `prueba 1` cuentan como el mismo). Si escribes un nombre que ya existe, el editor lo avisa y no deja guardarlo. Si ya tenías nombres repetidos de antes, el aviso aparece al abrir uno de ellos, y basta con cambiarle el nombre.
- **Renombrar o quitar:** con el cursor en el párrafo marcado, la banderita de la barra aparece activa. Púlsala para cambiar el nombre o quitar el marcador.
- **Enlazar:** escribe `@` y elige la página. Si tiene marcadores, el buscador pasa a un segundo paso con **Página completa** y sus marcadores. Si no tiene, el enlace va a la página directamente. `Esc` o borrar lo escrito vuelve a la lista de páginas. También funciona con el botón de enlazar.
- **En la misma página:** la página abierta aparece en el buscador como *esta página* si tiene marcadores, para enlazar a una parte de ella misma.
- **Ir:** el enlace muestra `Página › Marcador`, o solo el marcador si está en la misma página. Al hacer clic se abre la página, se desplaza hasta el párrafo y lo resalta un momento. El cursor queda al principio de ese párrafo.

El marcador pertenece al bloque entero, así que sigue funcionando aunque edites su texto. También resiste partir el párrafo con `Enter`, porque se queda en la parte que conserva texto, y unirlo con el de arriba o el de abajo. Ten en cuenta:

- Cada bloque admite un solo marcador. Si unes dos párrafos marcados, el bloque unido conserva el marcador del de arriba.
- Si borras el párrafo marcado, sus enlaces muestran solo el título de la página, con el subrayado discontinuo. Al hacer clic abren la página desde el inicio y avisan de que el marcador ya no existe.
- Al copiar y pegar un párrafo marcado, la copia no lleva el marcador.
- `Ctrl+Z` no deshace crear, renombrar ni quitar un marcador.
- En modo checklist no se ven. Si vuelves a **Texto libre** después de cambiar la lista, se pierden igual que el formato.

El enlace guarda el identificador de la página, no su posición, así que sigue funcionando aunque muevas o reordenes las páginas, y también después de exportar e importar una copia. Ten en cuenta:

- En modo checklist los enlaces se ven como texto. Si vuelves a **Texto libre** sin cambiar los elementos, regresan. Si los cambiaste, el texto vuelve sin formato y sin enlaces.
- Al renombrar una página, las que la enlazan muestran el nombre nuevo, pero el buscador de páginas las sigue encontrando por el nombre anterior hasta que las edites.
- Al pegar texto de una web, sus enlaces se quedan solo como texto.

## Navaja suiza

`navaja.html` reúne utilidades de uso diario en un tablero de cards. Escribe en el buscador (atajo `/`) para filtrar, pulsa una card para abrir la herramienta y vuelve con el botón de la izquierda o con `Esc`. Las últimas que usaste aparecen arriba, en **Recientes**.

| Grupo | Herramientas |
| --- | --- |
| Texto | Contador de texto · Mayúsculas y líneas · Comparar textos · Lorem ipsum · Markdown (vista previa en vivo, copiar HTML, guardar `.md` y `.html`) |
| Imágenes | Redimensionar · Recortar · Ajustes y filtros · Marca de agua · Convertir formato (PNG, JPEG, WebP, AVIF si el navegador lo admite, BMP e ICO) · Crear .ico · Comprimir imagen · Collage (de 2 a 9 fotos) · Firma a mano · Paleta de una imagen · Metadatos EXIF · Imagen ↔ Base64 |
| Codificación y seguridad | Codificar / decodificar (Base64, Base62, URL, HTML, Unicode, hex, binario) · Decodificar JWT · Hashes (MD5, SHA-1/256/384/512, HMAC) |
| Datos y código | JSON (formatear, minificar, validar, a YAML, JSON ↔ CSV) · Formatear SQL · Visor HTML (pega o abre una página y mírala funcionando, con consola y tamaños de móvil, tablet y escritorio) · Probador de regex · Bases numéricas · Expresiones cron |
| Generadores | UUID v4/v7 y contraseñas · Código QR (PNG y SVG) · Sorteo y equipos (elegir al azar, grupos y turnos) |
| Tiempo y color | Timestamp y fechas · Colores (HEX/RGB/HSL y contraste WCAG) |

Ten en cuenta:

- **Todo funciona sin conexión:** los archivos, imágenes y textos se procesan en tu navegador y no se envían a ningún lugar.
- **Los tokens son aproximados:** se estiman a partir de los caracteres y las palabras. El número exacto depende del tokenizador de cada modelo.
- **El JWT no se verifica:** la herramienta solo lee el header y el payload, no comprueba la firma.
- **Base62 usa el alfabeto `0-9`, `A-Z`, `a-z`:** es el más común. Algunas herramientas ponen las minúsculas antes que las mayúsculas, y con ellas el resultado no coincide.
- **Las herramientas de imagen comparten la imagen abierta:** ábrela una vez (soltándola, eligiéndola o pegándola con `Ctrl+V`) y pasa de una a otra sin volver a cargarla. Redimensionar, Recortar, Ajustes y filtros, Marca de agua, Convertir formato y Crear .ico son pestañas del mismo editor, así que los cambios se suman: el recuadro **Resultado** muestra cuáles están activos y se pueden quitar uno a uno. La vista previa trabaja sobre una copia reducida; al guardar se procesa la imagen a tamaño real. La imagen solo vive en memoria: al recargar la página hay que abrirla de nuevo.
- **Collage:** elige cuántas fotos quieres (de 2 a 9) y una de sus distribuciones, y carga las fotos con **Añadir fotos** (puedes elegir varias a la vez), soltándolas sobre el collage o pegándolas con `Ctrl+V`. La primera foto va al hueco más grande. Haz clic en un hueco para elegir su foto; arrastra la foto para encuadrarla y usa la rueda del ratón o el control **Zoom** para acercarla. Con las flechas ◀ ▶ la cambias de hueco. También eliges el formato (1:1, 4:5, 3:4, 4:3, 16:9 o 9:16), la separación, el redondeo de las esquinas, el color de fondo y el tamaño final. Los huecos vacíos salen del color de fondo. Las fotos no se mezclan con la imagen de las demás herramientas y, como ella, se pierden al recargar la página.
- **Firma a mano:** firma en el recuadro con el ratón, un lápiz o el dedo. Dentro del recuadro, el puntero es una cruz oscura con borde blanco, que se ve con cualquier tema de Windows, y un punto con el color y el grosor de la tinta marca dónde empieza el trazo. Con lápiz, el grosor sigue la presión; con **Trazo natural**, el ratón y el dedo hacen la línea más fina cuando vas rápido, como un bolígrafo. **Suavizado** (de 0 a 10; 5 por defecto) quita el temblor del ratón para que la línea quede pareja, sin mover el principio ni el final de cada trazo: en 0 la firma queda tal cual la trazaste. El color, el grosor y el suavizado se aplican a toda la firma, también a lo ya dibujado, así puedes firmar primero y ajustarlos después. **Deshacer** (`Ctrl+Z`) quita el último trazo y también recupera una firma borrada, y **Rehacer** (`Ctrl+Y`) lo vuelve a poner. **Guardar PNG** y **Copiar imagen** dan solo la zona firmada, a buena resolución y con fondo transparente (o blanco, si marcas **Fondo blanco**), lista para pegar en Word u otro documento. La firma se recuerda al volver; **Limpiar todo** la borra.
- **Sorteo y equipos:** escribe un nombre por línea o pega una columna de Excel. **Quitar repetidos** no distingue mayúsculas ni espacios de más. Todo el azar sale del generador seguro del navegador.
  - **Elegir al azar:** con **No repetir ganadores**, los elegidos salen del bombo hasta que pulses **Volver a meter a todos**. Cada sorteo queda en el historial con su fecha y hora.
  - **Hacer grupos:** reparte por número de grupos o por personas por grupo. Si no da exacto, los grupos se diferencian como mucho en una persona. **Copiar para Excel** pone un grupo por columna.
  - **Turnos:** el orden se sortea una vez y se repite en ciclo. Si añades a alguien, entra al final sin cambiar el resto; **Nuevo orden** lo vuelve a sortear. «Cada día hábil» salta sábados y domingos, pero no los festivos. «Cada mes» usa el mismo día del mes, o el último en los meses más cortos.
  - La lista, el historial, los grupos y el orden de turnos se recuerdan al volver.
- **Quitar metadatos no recomprime:** en JPEG, PNG y WebP se eliminan los bloques EXIF, XMP, IPTC, comentarios y textos tal cual, sin tocar los píxeles. El perfil de color se conserva.
- **El Visor HTML aísla la página:** se muestra en un marco sin acceso a la Navaja ni a tus datos. Los `console.log`, los errores y los `alert` salen en la consola de la herramienta; `localStorage` funciona, pero se borra en cada actualización. Las rutas relativas (imágenes, CSS o scripts junto a tu archivo) no se encuentran, porque la vista previa no sabe en qué carpeta está el archivo: usa direcciones completas o pon el contenido dentro del HTML. Con **Bloquear lo que venga de internet** no se carga nada de fuera.
- **Guardar archivos:** en Chrome y Edge, los botones **Guardar** (imágenes, `.txt`, `.sql`, `.md`, `.html`, JSON y QR en PNG o SVG) abren la ventana **Guardar como** y el botón queda en «Guardando…» hasta que el archivo se escribe. En otros navegadores se usa la descarga normal: el botón se bloquea 20 s y el mismo archivo pedido otra vez en menos de 30 s se ignora, para no bajar copias.
- Cada herramienta recuerda lo último que escribiste en ella, en `localStorage`. El botón **Limpiar todo**, arriba a la derecha de cada herramienta, borra ese texto, sus opciones y la imagen abierta (pide un segundo clic para confirmar). En las herramientas de imagen, **Quitar** cierra la imagen en todas ellas, y en la marca de agua también se puede quitar el logo.

Para agregar una herramienta, añade una llamada a `tool({ ... })` en el script de `navaja.html`, con su `id`, `cat`, `name`, `desc`, `color`, `icon` y una función `render(body, ctx)`.

## Tecnología

Mi día está hecho con HTML, CSS y JavaScript puros, sin frameworks, dependencias ni proceso de compilación. Para modificarlo, edita `midia.html` y recarga la página.

Usa estas APIs del navegador:

- **IndexedDB:** almacenamiento de los datos.
- **Web Workers:** reloj estable con la pestaña en segundo plano.
- **Web Locks:** bloqueo para mantener una sola pestaña activa.
- **Web Audio y Notifications:** avisos.
- **Document Picture-in-Picture:** mini ventana.

## Créditos

Desarrollado por [BYGUEN - Custom Development](https://www.byguen.com).

© 2026 BYGUEN. Todos los derechos reservados.

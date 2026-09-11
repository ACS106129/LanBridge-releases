# Registro de cambios

Aquí se recogen todos los cambios relevantes de LanBridge.

El formato sigue [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/) y el
versionado sigue [Versionado Semántico](https://semver.org/lang/es/).

## [0.5.5] - 2026-09-12

### Corregido

- **Las comprobaciones automáticas de actualización eran demasiado infrecuentes para
  parecer automáticas.** Cada cuatro horas significaba que, en la práctica, solo un
  reinicio encontraba algo, lo que dejaba un botón de la configuración como mecanismo real
  — y nadie quiere pulsar un botón para que le digan que no hay nada nuevo. Ahora se
  comprueba cada treinta minutos, y traer la ventana al frente también comprueba si la
  última fue hace más de cinco minutos. La configuración muestra cuándo se comprobó por
  última vez, para que se vea que ocurre.
- **Las dos opciones de confinamiento parecían duplicadas.** Ambas se expresaban como
  limitar el túnel, sin decir que limitan cosas distintas. Cada etiqueta nombra ahora su
  propio eje —*Solo las direcciones de la VPN pasan por el túnel* frente a *Solo la
  aplicación objetivo puede usar el túnel*— y cada explicación empieza diciendo a qué
  pregunta responde: qué destinos, o qué programa.

### Cambiado

- **El botón del aviso se llama *Actualizar ahora***, no *Novedades*. Lo que hace es
  instalar la actualización; mostrar las notas es algo que ocurre por el camino.
- **La configuración puede iniciar una actualización**, no solo buscarla.
- **La franja vacía de la parte superior de la ventana ha desaparecido.** La configuración
  y la información de la aplicación bajaron junto a las tarjetas de estado, que era lo
  único que había allí arriba.
- **El recuento de anuncios retransmitidos solo aparece con Warcraft III.** Es el único
  protocolo cuya información de partida hay que pedir y reenviar; en los demás el contador
  se quedaría en cero para siempre, lo que se lee como una avería y no como "no procede".

## [0.5.4] - 2026-09-12

### Corregido

- **La ventana de actualización mostraba las notas como su código Markdown** —almohadillas,
  asteriscos y comillas invertidas— en lugar de darles formato, lo que hacía penoso leer
  algo escrito precisamente para leerse. Ahora se formatean títulos, viñetas, énfasis y
  código en línea.
- **La actualización no cerraba la aplicación antes.** El instalador arrancaba mientras el
  túnel y el asistente con privilegios seguían reteniendo los archivos que iba a
  reemplazar. Ahora la sesión se detiene y este proceso termina antes de que el instalador
  se ejecute, y el instalador cierra cualquier instancia rezagada en vez de dejar que
  convierta una actualización en una petición de reinicio.
- **La ventana y la barra de tareas conservaban un icono genérico** mientras el área de
  notificación y Agregar o quitar programas mostraban el real. Una ventana sin empaquetar
  no toma el icono del ejecutable por sí sola.
- **La etiqueta china de «Mantener el resto del equipo fuera del túnel» describía el ajuste
  equivocado.** Se leía como «mantener fuera del túnel el tráfico de otras aplicaciones»,
  que es lo que hace el confinamiento por aplicación, y dejaba las dos opciones pareciendo
  duplicadas. Son ortogonales: una limita qué destinos usan el túnel, la otra qué proceso
  puede usarlo.

### Cambiado

- **El nombre y el lema ya no ocupan la parte superior de la ventana.** La barra de título
  ya dice qué es esto, y los detalles se han movido al cuadro de información.
- **El cuadro de información ya no repite el nombre con el que está titulado** y deja abrir
  la carpeta de registros al panel del registro, donde ese botón ya estaba. La versión, que
  es lo que se viene a consultar, se muestra ahora lo bastante grande para leerla de un
  vistazo.

## [0.5.3] - 2026-09-12

### Corregido

- **Una partida creada en una máquina se veía desde la otra pero no se podía entrar.** Solo
  se abría de entrada el puerto de descubrimiento, que no es necesariamente el puerto en el
  que escucha el anfitrión: Warcraft III toma el 6112 si puede y sube hasta el 6119 si no,
  y anuncia el que le haya tocado. Un anfitrión desplazado del 6112 quedaba visible e
  inalcanzable, y solo en ese sentido, lo que hacía parecer que el problema era de una de
  las dos máquinas. Ahora se abre todo el rango de alojamiento, siempre solo para la subred
  de la VPN.
- **Una partida seguía en la lista del otro jugador después de que el anfitrión la
  cerrara.** Warcraft III anuncia el cierre por difusión, y una difusión puede salir por el
  adaptador de la VPN, donde el relé deliberadamente no escucha: el anuncio nunca se
  recogía y el otro extremo seguía ofreciendo una partida que ya no existía. Ahora el relé
  advierte que el anfitrión ha dejado de responder a sus sondeos y la retira él mismo,
  usando el último anuncio que reenvió para decir cuál.
- **Cambiar de idioma vaciaba las listas de aplicación objetivo y de descubrimiento en
  red**, y elegir *Predeterminado del sistema* vaciaba la propia lista de idiomas.
  Retraducir una lista significa sustituir las entradas que contiene, y una lista
  desplegable interpreta la sustitución de la entrada seleccionada como su desaparición:
  borraba la selección, y la vinculación escribía ese vacío sobre la elección. Ahora las
  entradas conservan su identidad y solo cambia su texto, así que no queda nada que
  borrar. Dos intentos anteriores restauraban la selección después; este elimina la causa.
- **La ventana de ajustes mantenía el idioma anterior en su propio título y botón** cuando
  el idioma se cambiaba desde dentro. Todo el contenido de la ventana se volvía a
  etiquetar, pero el título y el botón de cerrar no forman parte de ese contenido.
- **El registro de actividad no seguía las líneas nuevas de forma fiable.** Se desplazaba
  antes de que la línea nueva se hubiera dispuesto, así que iba a donde estaba el final
  antes y se quedaba siempre una línea por detrás. Ahora se desplaza después de la
  disposición y deja de seguir en cuanto subes a leer algo, retomándolo al volver abajo.
- **Actualizar reescribía todos los archivos, hubieran cambiado o no.** La versión anterior
  se eliminaba por completo antes de escribir un solo archivo nuevo, así que cada
  actualización reescribía la instalación entera. Ahora se escribe primero la versión nueva
  y se elimina la anterior después, lo que permite al instalador omitir los archivos
  idénticos y dejar por escribir solo lo que realmente cambió.
- **Uno de los botones del registro estaba colocado y respondía al clic, pero no se
  dibujaba nunca.** *Abrir la carpeta de registros* ocupaba su sitio y reaccionaba a los
  clics sin mostrar absolutamente nada. Las acciones del registro están ahora en una sola
  fila horizontal en lugar de una columna cada una, lo que elimina la disposición por
  columnas que fallaba.

### Añadido

- **Un botón de información de la aplicación** junto al de ajustes: qué versión se está
  ejecutando, los derechos de autor y un enlace a sus notas y descargas.
- **Una casilla *Iniciar LanBridge* en la última página del instalador**, marcada de forma
  predeterminada. Arranca la aplicación sin elevación, que es como LanBridge debe
  ejecutarse: el consentimiento se pide al iniciar una sesión, no antes.

### Cambiado

- **Cerrar la aplicación objetivo ahora termina la sesión solo cuando el túnel está ligado
  a ella.** Con *Solo esta aplicación puede usar la VPN* activado, el túnel existe para ese
  proceso y cae con él; de lo contrario quedaría un túnel que nada en la máquina tiene
  permiso para usar. Sin esa opción, el túnel solo está acotado por destino y puede seguir
  llevando tráfico de otra cosa, así que se mantiene hasta que lo detengas.

## [0.5.2] - 2026-09-11

### Corregido

- **Las notas de la versión en la ventana de actualización mostraban solo su primer
  título.** Al extraerlas del registro de cambios, las notas se normalizan a saltos de
  línea simples, y un control de texto de Windows corta las líneas en el retorno de carro:
  todo lo que venía después de la primera línea no se dibujaba. El cuerpo de una versión en
  inglés ya trae retornos de carro, y por eso solo las notas traducidas parecían vacías.
  Ahora se convierten antes de mostrarse y aparecen en un bloque desplazable y
  seleccionable.

## [0.5.1] - 2026-09-11

### Corregido

- **Las partidas se veían pero no se podía entrar, o no aparecían en absoluto.** Todo lo
  que hace que una partida se pueda jugar llega *entrante* por el túnel, y Windows lo
  bloquea todo de forma predeterminada: el anuncio que reenvía el relé del compañero es
  UDP entrante, y entrar en la partida es una conexión TCP entrante. La versión de línea
  de órdenes abría ambos; la aplicación nunca lo hizo, así que la máquina que no
  conservara una regla suya quedaba inalcanzable en uno de los dos sentidos o en los dos.
  Ahora cada sesión abre el puerto de descubrimiento solo para la subred de la VPN —no
  para todas las redes a las que está conectada la máquina—, saca al adaptador del túnel
  de la categoría *pública* que Windows le asigna y deshace ambas cosas al terminar.
- **La interfaz se descuadraba con cualquier tamaño de texto mayor que el predeterminado,
  y reiniciar no la recuperaba.** La capa que se amplía se centraba primero y luego crecía
  desde su propia esquina superior izquierda, así que todo empezaba más abajo y más a la
  derecha de lo debido y se salía por el borde inferior y el derecho, llevándose consigo
  el botón de configuración. Como el tamaño del texto se recuerda, cada reinicio volvía al
  mismo estado roto sin forma de llegar al ajuste que lo causaba.
- **La descarga de una actualización no mostraba ningún progreso.** La ventana se cerraba
  en cuanto se pulsaba *Descargar* y la transferencia ocurría sin nada en pantalla, algo
  indistinguible de una descarga que nunca empezó. Las notas de la versión ahora siguen
  abiertas, con una barra de progreso, la cantidad transferida y un *Cancelar* que
  funciona.
- **Volver a activar la comprobación automática de actualizaciones no hacía nada durante
  hasta cuatro horas.** La comprobación en segundo plano solo revisaba el ajuste en su
  siguiente ejecución programada; ahora mira de inmediato.
- **La elección de qué hacer al cerrar la ventana parecía descartarse** al cambiar el
  idioma en la misma visita a Configuración. Traducir la lista sustituye la entrada
  seleccionada, lo que borra la selección: las demás listas se recuperan solas, esta no.

### Añadido

- **Las comprobaciones de actualización también se ejecutan con la aplicación en el área
  de notificación**, cada cuatro horas en vez de solo al arrancar. Una versión nueva se
  anuncia con un globo en el área de notificación, y la información sobre el icono lo
  sigue indicando cuando el globo desaparece.
- **Un botón *Comprobar ahora* en Configuración**, para cuando esperar a la siguiente
  comprobación programada no tiene sentido.

## [0.5.0] - 2026-09-11

### Corregido

- **La VPN se cortaba nada más abrir una partida.** La búsqueda del proceso al que cede el
  paso un lanzador solo comparaba ejecutables con el mismo nombre, así que un juego que
  continúa con otro nombre nunca se encontraba y el túnel caía. Ahora cuenta cualquier
  proceso que siga ejecutándose desde la misma carpeta de instalación, y la búsqueda deja
  constancia de qué buscó.
- **Detener no hacía nada una vez terminada la sesión.** Cerrar la tubería hacia el proceso
  auxiliar lanzaba un error cuando el otro extremo ya no existía, y ese error escapaba de
  la limpieza, dejando a la aplicación convencida de que una sesión terminada seguía activa.
- **Cambiar de idioma vaciaba todas las listas desplegables** en lugar de traducirlas.
  Sustituir el contenido de una lista borra la selección, y el enlace escribía de vuelta esa
  selección vacía.
- **El registro de actividad no seguía las líneas nuevas.** Ahora se desplaza hasta la más
  reciente y deja de seguirlas en cuanto subes para leer algo.

### Añadido

- **Notas de la versión en tu idioma.** Los registros de cambios traducidos se publican
  junto a las compilaciones, y el diálogo de actualización muestra el que corresponde al
  idioma de la interfaz.
- **Exportar informe de errores**: un botón que aparece con un indicador en cuanto algo
  falla. Reúne la actividad en pantalla con los archivos de registro de ambos procesos, de
  modo que se pueda informar de un problema sin saber dónde se guardan.
- El ajuste de tamaño del texto ahora escala toda la interfaz, no solo el registro.

## [0.4.0] - 2026-09-10

### Añadido

- **Comprobación automática de actualizaciones.** La aplicación consulta si hay una
  versión más reciente en GitHub y muestra qué cambió antes de instalarla. Activada por
  defecto; se puede desactivar en Ajustes.
- **Diálogo de ajustes** tras el botón del engranaje, con el idioma, el tamaño del texto
  de la interfaz, el comportamiento al cerrar y la comprobación de actualizaciones, de
  modo que una elección recordada nunca sea un callejón sin salida.
- **Diez idiomas más**: chino tradicional, chino simplificado, japonés, coreano, francés,
  alemán, portugués, ruso e italiano, junto al inglés y el español. El estado, las listas
  desplegables, los diálogos y el menú del área de notificación están traducidos.
- **El registro de actividad ahora es texto seleccionable**, con botones *Copiar todo* y
  *Exportar…*.
- **Tamaño del texto de la interfaz ajustable** (10–22 pt), recordado entre sesiones.
- **Icono de la aplicación**, usado por la ventana, la barra de tareas, el área de
  notificación y Agregar o quitar programas.
- **El instalador pregunta dónde instalar** y ofrece el acceso directo del menú Inicio y el
  del escritorio como opciones independientes.

### Corregido

- **La VPN se cortaba justo cuando el juego terminaba de cargar.** Warcraft III, como la
  mayoría de los títulos con un lanzador o un actualizador, cierra su primer proceso y cede
  el paso a otro. Ese cierre se interpretaba como «la aplicación se ha cerrado» y se
  desmontaba el túnel en el peor momento. Ahora la sesión sigue a la aplicación a través
  del relevo.
- **El icono del área de notificación no aparecía nunca.** Su identificador se destruía
  antes de que Windows lo usara, y `Shell_NotifyIcon` con un identificador destruido no
  muestra nada ni informa de ningún error.
- **Reabrir desde el área de notificación iniciaba una segunda copia** en lugar de
  restaurar la que ya estaba en marcha. Ahora se ejecuta una sola instancia por usuario y
  volver a abrirla trae la ventana existente al frente.
- **`WinDivert64.sys` quedaba bloqueado tras cerrar la aplicación.** Cerrar los
  identificadores del controlador no basta: al abrir uno se registra un servicio de núcleo
  que sigue en marcha, y el archivo permanece bloqueado hasta detenerlo. Ahora el servicio
  se detiene y se elimina al terminar la sesión. Cerrar la ventana también detiene el
  proceso auxiliar con privilegios, cosa que antes no ocurría.
- **Volver el idioma a «Predeterminado del sistema» no hacía nada.** El cambio se anunciaba
  con una única notificación de «todo ha cambiado», que WinUI no atiende de forma fiable;
  ahora cada cadena se anuncia por su nombre.
- **Desinstalar pedía reiniciar.** El instalador cierra primero la aplicación y su auxiliar,
  así que no quedan archivos en uso.
- Las filas del registro tenían el relleno de un elemento de lista, que dejaba media línea
  en blanco entre entradas.

### Cambiado

- Las acciones del registro se han movido al panel de actividad de la derecha, en lugar de
  quedar al final de la columna de configuración.

## [0.3.0] - 2026-09-09

### Añadido

- Registro de errores en `%LOCALAPPDATA%\LanBridge\logs\`, un archivo por proceso y
  ejecución, con las excepciones no controladas capturadas en tres puntos y registradas en
  lugar de cerrar la aplicación en silencio.
- Persistencia de los ajustes: se escriben en cada cambio, así que sobreviven a un fallo o
  a un cierre forzado.
- Área de notificación, con una pregunta al cerrar para salir o minimizar.
- Interfaz en chino tradicional junto al inglés.
- Instalador MSI con accesos directos, información de versión y entrada en Agregar o quitar
  programas.

### Corregido

- La compilación publicada se iniciaba y moría de inmediato dentro del entorno XAML:
  al publicar una aplicación WinUI sin empaquetar no se incluye su marcado compilado, así
  que `InitializeComponent` no encontraba nada que cargar.

## [0.2.0] - 2026-09-09

### Corregido

- **La ventana no aparecía nunca tras el aviso de elevación.** WinUI 3 no puede ejecutarse
  con privilegios elevados: la activación WinRT falla y el proceso termina sin mostrar
  nada. Ahora la interfaz se ejecuta sin privilegios y delega el trabajo privilegiado en un
  proceso auxiliar aparte, que pide consentimiento una vez por sesión. La interfaz no tiene
  privilegios y el auxiliar solo los mantiene mientras hay una sesión activa.

## [0.1.0] - 2026-09-09

### Añadido

- VPN por aplicación: importa un perfil de OpenVPN y rechaza la ruta predeterminada y el
  DNS que envía el servidor, de modo que solo la subred VPN cruza el túnel y el resto del
  equipo conserva su ruta habitual.
- Confinamiento opcional por proceso mediante WinDivert, descartando el tráfico hacia la
  subred VPN de cualquier proceso que no sea el elegido.
- Retransmisión de la detección LAN para túneles que no pueden llevar difusión, incluido el
  protocolo W3GS de Warcraft III, cuya información de partida solo se envía como respuesta
  unidifusión y nunca por difusión, por lo que hay que pedírsela al juego local y
  reenviarla.
- Retransmisión genérica de difusión UDP para otros juegos, configurada por puerto.
- Versión de línea de comandos del mismo motor.

[0.5.5]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.5
[0.5.4]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.4
[0.5.3]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.3
[0.5.2]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.2
[0.5.1]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.1
[0.5.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.0
[0.4.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.4.0
[0.3.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.3.0
[0.2.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.2.0
[0.1.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.1.0

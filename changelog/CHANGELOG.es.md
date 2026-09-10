# Registro de cambios

Aquí se recogen todos los cambios relevantes de LanBridge.

El formato sigue [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/) y el
versionado sigue [Versionado Semántico](https://semver.org/lang/es/).

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

[0.5.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.0
[0.4.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.4.0
[0.3.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.3.0
[0.2.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.2.0
[0.1.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.1.0

# Registro de cambios

Todos los cambios notables de LanBridge se registran aquí.

El formato sigue [Keep a Changelog](https://keepachangelog.com/es/1.1.0/) y las versiones
siguen el [versionado semántico](https://semver.org/lang/es/).

## [0.5.34] - 2026-09-26

### Cambiado

- El registro muestra primero la línea más reciente.
- Solo se muestran los controles que usan las opciones actuales.
- El parche de traducción tiene su propia tarjeta.
- Solo los clics en botones y menús suenan.

### Corregido

- Elegir un archivo que no es parche mantiene el estado.

## [0.5.33] - 2026-09-25

### Añadido

- El modo DMM carga o quita parches de traducción.

### Cambiado

- Registros en texto plano, una conclusión por línea.

### Corregido

- Un servidor mudo se señala como causa.

## [0.5.32] - 2026-09-24

### Añadido

- DMM Game Player se instala e inicia en un paso.
- Orden por país, velocidad, latencia o sesiones.
- Los perfiles guardan su velocidad.
- La descarga muestra velocidad y tiempo.

### Cambiado

- Los sitios del túnel, junto al perfil VPN.
- Las actualizaciones bajan mucho más rápido.
- Cada ayuda es una sola frase.

### Corregido

- Un túnel caído ya no figura como listo.
- Un mismo perfil se guarda una sola vez.
- El botón quitar ya no queda bajo la barra.

## [0.5.31] - 2026-09-23

### Añadido

- Una lista de sitios de DMM lista para usar.

### Cambiado

- Solo el inicio de sesión pasa por el túnel; el juego ya no se ralentiza.
- La sesión ya no persigue a la aplicación hasta otro proceso.
- Si la aplicación ya está abierta, se usa esa en vez de abrir otra.

### Corregido

- El túnel ya no muere en silencio tras una reconexión.
- La dirección del inicio de sesión ya no se pierde entre dos consultas.
- Un corte breve ya no reinicia el túnel.

## [0.5.30] - 2026-09-20

### Corregido

- Una comprobación rechazada ya no se muestra como actualizada.
- Se pueden borrar perfiles sin usar durante la conexión.
- El modo de espera ya no dice que está abriendo.
- Los sitios no listados ya no salen del túnel.
- Las líneas que se repiten a intervalos vuelven al registro.

## [0.5.29] - 2026-09-19

### Añadido

- Descargar una VPN desde VPN Gate.
- Cambiar la lista de sitios en conexión.

### Cambiado

- En el túnel sólo los sitios del destino.
- La actualización trae un solo runtime.

### Corregido

- El seguimiento sobrevive a la reconexión.
- Las comprobaciones de actualización ya no se rechazan.

## [0.5.28] - 2026-09-19

### Corregido

- **La actualización es 38,8 MB más pequeña**: el conjunto de aprendizaje automático del Windows App SDK, onnxruntime y DirectML, viajaba en una VPN por aplicación que nunca lo llama.
- **Una publicación ya no es visible hasta que todos sus instaladores están adjuntos**, que es por lo que 0.5.27 ofreció un instalador en alemán a una interfaz en chino: se publicó con un archivo subido y el resto aún en camino, y la aplicación detecta una versión nueva en aproximadamente un minuto.

## [0.5.27] - 2026-09-19

### Corregido

- **La aplicación objetivo ya no alcanza nada salvo a través del túnel**: el primer paquete de una conexión a una dirección que ninguna ruta cubre se descarta en lugar de salir con la dirección real de esta máquina, que es lo que provocaba los 403 repetidos y la pantalla de carga atascada.
- **Descartar ese paquete es lo que añade la ruta**, de modo que la conexión prospera en su primera retransmisión en vez de fallar.
- **Un paquete IPv6 bloqueado ya no se cuenta ni se registra como fuga**, ni en la línea del momento ni en el veredicto final, que describía una ejecución medida como "22 de 73 destinos no pasaron por el túnel" cuando los 51 de IPv4 sí pasaron y los 22 eran el guardián funcionando según lo previsto.
- **Se sigue al túnel cuando se reconecta con otra dirección**, y si la nueva queda fuera de la subred sobre la que se construyó el filtro de paquetes, se deja de rechazar y se dice, en lugar de convertir en rechazo cada paquete que envía el objetivo.

### Cambiado

- **Las rutas vuelven a salir del túnel**: una dirección se libera cuando ningún nombre seguido la responde ya y el objetivo no tiene conexiones abiertas hacia ella, en lugar de crecer el conjunto durante toda la sesión.
- **Una ruta adoptada caduca tras cinco minutos sin uso** y devuelve su plaza dentro del límite de la sesión.

## [0.5.26] - 2026-09-18

### Cambiado

- **La lista de sitios ya no tiene que ser correcta**: ahora es solo un arranque en caliente, y todo lo que el objetivo alcanza sin el túnel recibe su propia ruta, salvo el servidor VPN, las redes propias de esta máquina, la difusión y IPv6.
- **El resumen final ya no llama perdido a un destino después de haberlo enrutado**, porque vuelve a preguntar al sistema en lugar de releer las rutas añadidas.

## [0.5.25] - 2026-09-17

### Corregido

- **Los sitios solo pasaban por el túnel cuando ambos lados coincidían por casualidad en su dirección**: dieciocho de veintisiete nombres respondían distinto, solo se enrutaba la respuesta del túnel y la aplicación usaba la local, así que ahora se enrutan ambas.
- **Los sitios se editan uno por fila, en una ventana propia**, en vez de una caja con veintisiete nombres.
- **Una descarga que el servidor corta se retoma donde se detuvo** en lugar de tirarse, pidiendo cada parte hasta cinco veces.
- **0.5.24 culpó de eso a un tiempo de espera y se equivocó**: el fallo llevaba treinta minutos, así que un límite de quince no pudo terminarlo.

## [0.5.24] - 2026-09-17

### Corregido

- **Una descarga lenta se tiraba justo antes de acabar**, porque el límite de quince minutos cubría la lectura del archivo y no solo llegar al servidor; ahora no hay límite global.
- **Las actualizaciones bajan unas tres veces más rápido**, porque el servidor de publicación limita cada conexión y no la línea, así que el instalador se obtiene en cuatro partes a la vez.
- **El progreso se informa cada 512 KB en lugar de cada 80 KB**, que es un ritmo que la ventana puede aprovechar.

## [0.5.23] - 2026-09-16

### Añadido

- **La sesión anota adónde fue realmente el objetivo y qué parte se saltó el túnel**, de modo que una ejecución nombra lo que falta en vez de dejar la lista de nombres en conjeturas.
- **Las direcciones se informan con el nombre al que responden**, porque el nombre de borde de una red de contenidos lleva la ubicación, y la ubicación es toda la cuestión.

### Corregido

- **Los nombres configurados se siguen mientras dura la sesión**, en lugar de fijarse una vez al inicio, ya que responden con un TTL de sesenta segundos y una sesión dura horas.
- **La búsqueda de un resolutor que funcione se detiene en cuanto uno responde**, sin agotar tiempos por cada nombre y costar minutos antes de empezar.

## [0.5.22] - 2026-09-16

### Añadido

- **La dirección de salida se mide antes y después de poner las rutas, y ambas respuestas van al registro**, porque toda comprobación anterior era un paso hacia eso y no eso mismo — y "no se pudo saber" se escribe tal cual.

### Corregido

- **El registro conserva ya la mitad de la sesión para la que haría falta**: los pasos de la ventana, cada mensaje del filtro de paquetes y todo lo que dijo OpenVPN van al archivo.
- **Una ruta ya no se rechaza un instante antes de empezar a funcionar**; se dan unos cientos de milisegundos a la tabla de rutas para asentarse.
- **El filtro de paquetes anota el filtro con el que se abrió**, para distinguir una cláusula ausente de una que nunca coincidió.

## [0.5.21] - 2026-09-14

### Corregido

- **Los sitios enviados por la VPN no iban por ella mientras todo decía que sí**: el siguiente salto se adivinaba como el .1 de la red, que en un /30 no existe, así que ahora se deduce de la dirección que el túnel obtuvo y cada ruta se verifica como la que el sistema usaría de verdad.
- **Un nombre que no se pudo resolver por el túnel se informaba como coincidente con la respuesta local**; no hay respuesta no es la misma respuesta, y el registro dice qué resolutor y qué transporte la produjeron.
- **La consulta por el túnel recurre a TCP**, porque un relé que transporta uno y no el otro es habitual en servidores voluntarios.

## [0.5.20] - 2026-09-13

### Añadido

- **Sitios que puede enviar por la VPN sin enviar la máquina entera**: los sitios nombrados se resuelven por el túnel y se enrutan por él, y durante la sesión solo los alcanza la aplicación objetivo.

## [0.5.19] - 2026-09-13

### Añadido

- **Un lugar para el usuario y la contraseña**, para servidores que piden identificarse; el diálogo dice sin rodeos que openvpn solo puede leerla de un archivo, así que se guarda sin cifrar en la carpeta propia de ese perfil.

### Corregido

- **El objetivo alcanzaba internet por IPv6, rodeando el túnel por completo**: una fuga en todos los modos, ya que un túnel que transporta IPv4 no puede transportar lo que la máquina envía por IPv6, así que ahora el IPv6 del objetivo se descarta.
- **"Sin actualizaciones" cuando no se había preguntado nada**: con el límite agotado informaba de la versión que ya tenía, y el validador que hace gratuita la comprobación se conserva ahora entre ejecuciones.

## [0.5.18] - 2026-09-13

### Corregido

- **El cuadro Acerca de agradecía a WinDivert sin decir bajo qué términos se usa**, y ahora nombra la LGPL v3, la copia que se distribuye junto al programa y dónde está el código.
- **Las plazas libres de una sala de Warcraft III nunca cambiaban en la otra máquina**, porque el aviso que lee el otro extremo es difusión y no se puede capturar, así que se deriva del anuncio y se comprueba antes de enviarlo.
- **La descarga de la actualización seguía reteniendo la ventana**: 0.5.16 dijo que estaba arreglado mientras nada llamaba a ese código; ahora corre de verdad en segundo plano, con **Continuar en segundo plano** en el diálogo y cancelación en la ventana principal.

## [0.5.17] - 2026-09-13

### Corregido

- **Que un jugador saliera de la sala de Warcraft III la cerraba para todos**, porque el escuchador y cada conexión aceptada compartían un registro de puerto y el primer cierre se lo llevaba; ahora cada socket se sigue por separado.
- **La aplicación objetivo seguía ejecutándose como administrador**: la vía de 0.5.16 requería un privilegio que un ayudante elevado no puede tener, así que se usa el que sí tiene y el registro dice cuál.

## [0.5.16] - 2026-09-13

### Añadido

- **Un instalador en cada idioma que habla la aplicación**, cada uno con la página de códigos ANSI de su cultura, y la actualización ofrece el que coincide con el idioma de la ventana.
- **La ventana se abre donde la dejó**, salvo que esa posición ya no caiga en ninguna pantalla.
- **Un icono nuevo**: una flecha que sale por la abertura de un anillo, dibujado por separado en cada tamaño en vez de reducido desde una imagen grande.

### Corregido

- **Iniciar quedaba por debajo del pliegue**; los botones están ahora fijados bajo las tarjetas, que se desplazan tras ellos.
- **La aplicación objetivo se ejecutaba como administrador**, de modo que un inicio de sesión en el navegador nunca podía entregarle el código de autorización; ahora se lanza con el token del shell.
- **Descargar una actualización secuestraba toda la ventana**, y ahora corre en segundo plano con barra de progreso y un botón de cancelar que funciona.

## [0.5.15] - 2026-09-13

### Corregido

- **Un solo rechazo del servidor terminaba el intento**, lo correcto para un servidor propio y erróneo para un relé público, así que reintenta tres veces antes de informar.
- **"EXITING auth-failure" no explicaba nada** y ahora dice qué paso falló y qué significa para el tipo de servidor que usa.

## [0.5.14] - 2026-09-12

### Añadido

- **Un perfil importado pasa a ser de la aplicación**: el .ovpn y cada certificado y clave a los que hace referencia se copian a una carpeta propia, así que borrar el original no cambia nada.
- **Un lugar para ver qué se guarda**, con renombrar, borrar y una vía a la carpeta, confirmando dentro de la propia fila.
- **OpenVPN, si no lo tiene**, obtenido del servidor de descargas del propio OpenVPN y rechazando cualquier cosa en la que Windows no confíe o que OpenVPN no haya firmado.
- **Pruebas para el instalador**, que recorren sus páginas en ambos idiomas y cancelan en el resumen, de modo que ejecutar la suite no instala nada.
- **Una prueba que mira los píxeles**, midiendo cada línea explicativa de Ajustes y Acerca de contra su fondo en ambos temas.

### Corregido

- **Tres líneas de Acerca de eran invisibles**, porque un pincel tomado de los recursos de la aplicación se resuelve con el tema de la propia aplicación, que una aplicación WinUI sin empaquetar no puede cambiar tras arrancar.
- **La comprobación de actualizaciones dejó de pedir con educación**: ahora lee cuándo vuelve el cupo y espera, y lo dice una vez en lugar de sesenta.
- **El instalador escribía encima de su propia ilustración**, que es el fondo sobre el que escribe el diálogo y no una imagen al lado del texto.
- **La actualización daba a todo el mundo el instalador en inglés**, y ahora pide el que coincide con el idioma de la ventana.

### Cambiado

- Cada ajuste tiene debajo una línea que dice qué cambia y dónde se guardan los ajustes.
- Acerca de dice cada cuánto se buscan actualizaciones y acredita al cliente comunitario de OpenVPN y a WinDivert.

## [0.5.13] - 2026-09-12

### Añadido

- **Un piano de cola**: los sonidos de los controles suenan por el sintetizador General MIDI que Windows ya tiene, en escala pentatónica para que dos notas cualesquiera casen, con una casilla para callarlo.
- **Movimiento donde ha pasado algo**, y no en todas partes.
- **El diagrama informa en lugar de actuar**: quieto mientras no hay sesión, y la celda de bloqueados muestra los paquetes que el guardián descartó de verdad.
- **Un instalador que se parece a este producto**, con ilustraciones generadas en vez de los marcadores de WiX.
- **Un instalador en su idioma**, un MSI por idioma, para empezar inglés y chino tradicional.

### Cambiado

- El aviso bajo el confinamiento por proceso aparece ahora cuando la casilla está **vacía**, que es el estado sobre el que merece la pena avisar.

## [0.5.12] - 2026-09-12

### Corregido

- **La carpeta de instalación tenía ochenta y ocho carpetas de traducciones de idiomas que esta aplicación no ofrece**, incluidas como recursos Win32 que el ajuste habitual de recorte no alcanza; ahora quedan quince.

### Cambiado

- La opción por proceso se llama *Solo la aplicación objetivo puede hablar con la VPN*, que es lo que hace; nunca gobernó el túnel entero.

### Añadido

- **Diez comprobaciones más que manejan la ventana real**, sobre los diálogos y las opciones dentro de ellos — y escribirlas encontró cuatro pruebas que buscaban algo que nunca existió.

## [0.5.11] - 2026-09-12

### Corregido

- **El modo oscuro era texto blanco sobre página blanca**, porque el tema se aplicaba a un elemento dentro del que pinta el fondo.
- **El icono de la sección de destino se dibujaba como una caja vacía**, ya que un punto de código en el mapa de caracteres de una fuente no significa que haya glifo.
- **Los controles de cada sección quedaban centrados** en una tarjeta que ya tenía el ancho correcto.
- **Elegir adjuntar y pulsar iniciar sin escoger programa lanzaba una excepción**, y ahora dice qué elección falta.

### Añadido

- **Pruebas que manejan la ventana real**: diecinueve comprobaciones que abren la compilación publicada y miran, porque todos los defectos visuales reportados hasta ahora podían pasar todas las pruebas unitarias del proyecto.

## [0.5.10] - 2026-09-12

### Corregido

- **La línea de aviso ensanchaba la columna en lugar de ajustarse**, porque una pila horizontal mide a sus hijos con ancho ilimitado.
- **Ajustes e información de la aplicación aparecían dos veces**, al quedarse el par antiguo cuando se movieron arriba a la derecha.
- **El selector de procesos ofrecía esta misma aplicación.**

### Cambiado

- **El diagrama llena el espacio que se le da**, en vez de quedarse con ancho fijo en la esquina de una tarjeta grande y vacía.
- **La nota bajo el confinamiento por proceso solo aparece mientras está activo**, y dice lo que hace.
- La ruta del objetivo se muestra completa al pasar el ratón, por estrecha que sea la caja.

## [0.5.9] - 2026-09-12

### Corregido

- **El modo oscuro era inservible**, porque la página nunca pintaba un fondo propio y a los diálogos había que indicarles el tema aparte.

### Añadido

- **Los dos interruptores de confinamiento son ahora una imagen**, una rejilla de quién envía contra hacia dónde, que responde de un vistazo lo que dos párrafos no lograban.
- **Adjuntar elige de una lista de programas en ejecución**, en vez de pedir un identificador de proceso copiado de otro sitio.

### Cambiado

- El aviso de enrutamiento aparece solo mientras esa opción está activa, dice una cosa y la dice en el color de un aviso.
- Las secciones perdieron su numeración; nunca fueron pasos a seguir en orden.
- Ajustes e información de la aplicación se movieron arriba a la derecha, con las tarjetas de estado debajo.

## [0.5.8] - 2026-09-12

### Corregido

- **Confinar el túnel a una aplicación solo funcionaba en un sentido**, dejando alcanzable desde el otro extremo a todos los demás programas de aquí, que es la mitad que importa cuando no sabe quién está al otro lado.

### Cambiado

- **La opción de enrutamiento está ahora del revés**: *Enviar todo el tráfico por la VPN*, desactivada por omisión, diciendo lo que activarla significa para quien opere ese servidor.

### Añadido

- **Apariencia clara y oscura**, o siguiendo al sistema, aplicada al momento y recordada.
- **Iconos por toda la interfaz** y una luz de estado verde mientras hay sesión.

## [0.5.7] - 2026-09-12

### Corregido

- **Una descarga no se podía cancelar**, porque corría mientras retenía el aplazamiento de clic del diálogo, lo que hace que el diálogo desactive sus propios botones.
- **Una descarga cancelada o fallida dejaba su archivo parcial**, uno por intento, para siempre.
- **Pulsar Iniciar sin nada que iniciar no hacía absolutamente nada**, y ahora dice qué elección falta.

### Cambiado

- **Instalar una actualización con una sesión en marcha avisa primero**, con la respuesta segura por omisión, ya que instalar desconecta a la aplicación objetivo.

## [0.5.6] - 2026-09-12

### Corregido

- **Una versión nueva se detecta ahora aproximadamente un minuto después de publicarse**, con una petición condicional que no cuesta nada mientras no cambia.
- **Descartar el aviso de actualización no dejaba forma de volver a él**; ajustes y el diálogo de información ofrecen ahora *Actualizar ahora* mientras haya una esperando.
- **Detener decía *Detener* cuando ya no quedaba nada que detener**, y dice *Forzar detención* durante la espera del relevo.

### Cambiado

- **Todo lo relativo a actualizar está ahora en el diálogo de información**, junto a la versión con la que se compara.
- **Un instalador descargado se conserva si elige *Más tarde***, hasta que su versión quede superada.

## [0.5.5] - 2026-09-12

### Corregido

- **Las comprobaciones automáticas eran demasiado raras para parecer automáticas**: ahora cada treinta minutos, y también al traer la ventana al frente si la última supera los cinco minutos.
- **Las dos opciones de confinamiento se leían como duplicadas**, y cada etiqueta nombra ahora su propio eje: qué destinos, o qué programa.

### Cambiado

- **El botón del banner se llama *Actualizar ahora*** en vez de *Novedades*, porque instalar es lo que hace.
- **Ajustes también puede iniciar una actualización**, no solo buscarla.
- **La franja vacía en lo alto de la ventana ha desaparecido.**
- **El recuento de anuncios retransmitidos solo se muestra para Warcraft III**, el único protocolo al que se aplica.

## [0.5.4] - 2026-09-12

### Corregido

- **La ventana de actualización mostraba las notas como código Markdown** en lugar de darles formato, lo que hacía penoso leer algo escrito para ser leído.
- **Actualizar no cerraba antes la aplicación**, así que el instalador iba a sustituir archivos que el túnel y el ayudante seguían reteniendo.
- **La ventana y la barra de tareas conservaban un icono genérico**, porque una ventana sin empaquetar no toma el icono del ejecutable por sí sola.
- **La etiqueta china de "mantener el resto de la máquina fuera del túnel" describía el otro ajuste**, lo que hacía parecer duplicadas dos opciones ortogonales.

### Cambiado

- **El nombre y el lema ya no ocupan lo alto de la ventana**; la barra de título ya dice qué es esto.
- **El diálogo de información ya no repite el nombre de su propio título**, y muestra la versión lo bastante grande para leerla de un vistazo.

## [0.5.3] - 2026-09-12

### Corregido

- **Una partida alojada en una máquina se veía pero no se podía unir desde la otra**, porque solo se abría el puerto de descubrimiento mientras Warcraft III sube hasta 6119 cuando 6112 está ocupado; ahora se abre todo el rango, siempre solo a la subred VPN.
- **Una sala seguía en la lista del otro jugador después de que el anfitrión la dejara**, porque el aviso de cierre es difusión y nunca llega, así que el relé nota que el anfitrión ha dejado de responder y la retira él mismo.
- **Cambiar de idioma vaciaba las listas de aplicación objetivo y descubrimiento LAN**, porque sustituir la entrada seleccionada se lee como su desaparición; ahora las entradas mantienen su identidad y solo cambia su texto.
- **La ventana de ajustes conservaba el idioma antiguo en su título y su botón**, que no forman parte del contenido reetiquetado.
- **El registro de actividad no seguía con fiabilidad las líneas nuevas**, porque se desplazaba antes de que la nueva línea estuviera dispuesta.
- **Actualizar reescribía todos los archivos, hubieran cambiado o no**, porque la versión antigua se eliminaba entera antes de escribir un solo archivo nuevo.
- **Uno de los botones del registro estaba dispuesto y era pulsable, pero nunca se dibujaba.**

### Añadido

- **Un botón de información de la aplicación** junto al de ajustes, con la versión, el copyright y un enlace a las notas de esa versión.
- **Una casilla *Iniciar LanBridge* en la última página del instalador**, que lo arranca sin elevación, que es como está pensado.

### Cambiado

- **Cerrar la aplicación objetivo termina la sesión solo cuando el túnel está ligado a ella**, ya que si no podría seguir transportando tráfico de otra cosa.

## [0.5.2] - 2026-09-11

### Corregido

- **Las notas en la ventana de actualización mostraban solo su primer encabezado**, porque el control corta líneas en un retorno de carro que las notas normalizadas ya no llevaban.

## [0.5.1] - 2026-09-11

### Corregido

- **Las partidas se veían pero no se podían unir, o no aparecían**: todo lo que hace unible una partida llega de entrada y Windows lo bloquea por omisión, así que una sesión abre el puerto de descubrimiento solo para la subred VPN y lo deshace todo al terminar.
- **La interfaz se descomponía con cualquier tamaño de texto por encima del normal, y reiniciar nunca la devolvía**, porque la capa escalada se centraba y después crecía desde su propia esquina superior izquierda.
- **Descargar una actualización no mostraba progreso alguno**, indistinguible de una descarga que nunca empezó.
- **Volver a activar las comprobaciones automáticas no hacía nada durante hasta cuatro horas.**
- **La elección de cierre parecía descartada** si se cambiaba el idioma en la misma visita a Ajustes.

### Añadido

- **Las comprobaciones siguen mientras la aplicación está en el área de notificación**, anunciadas con un globo y conservadas en la información del icono.
- **Un botón *Comprobar ahora* en Ajustes**, para cuando esperar a la siguiente no es la cuestión.

## [0.5.0] - 2026-09-11

### Corregido

- **La VPN caía en cuanto se abría una sala**, porque la búsqueda del proceso al que un lanzador cede solo emparejaba ejecutables con el mismo nombre.
- **Detener no hacía nada si la sesión ya había terminado**, porque cerrar la tubería hacia un ayudante desaparecido lanzaba una excepción que escapaba de la limpieza.
- **Cambiar de idioma vaciaba todas las listas** en lugar de traducirlas.
- **El registro de actividad no seguía las líneas nuevas**, y ahora deja de seguir en cuanto se desplaza hacia arriba.

### Añadido

- **Notas de la versión en su idioma**, publicadas junto a las compilaciones y mostradas según el idioma de la interfaz.
- **Exportar informe de error**, que junta la actividad en pantalla con los registros de ambos procesos.
- El ajuste de tamaño de texto escala ahora toda la interfaz, no solo el registro.

## [0.4.0] - 2026-09-10

### Añadido

- **Comprobación automática de actualizaciones**, mostrando qué cambió antes de instalar; activada por omisión.
- **Diálogo de ajustes** tras el botón del engranaje, con idioma, tamaño de fuente, comportamiento al cerrar y comprobación de actualizaciones.
- **Diez idiomas más de interfaz**, junto al inglés y el chino tradicional.
- **El registro de actividad es ahora texto seleccionable**, con *Copiar todo* y *Exportar…*.
- **Tamaño de fuente ajustable** (10–22 pt), recordado entre ejecuciones.
- **Icono de la aplicación**, usado por la ventana, la barra de tareas, el área de notificación y Agregar o quitar programas.
- **El instalador pregunta dónde instalar** y ofrece los dos accesos directos como opciones independientes.

### Corregido

- **La VPN caía en cuanto la partida terminaba de cargar**, porque la salida del primer proceso del lanzador se leía como el cierre del objetivo; ahora la sesión sigue el relevo.
- **El icono de la bandeja no aparecía nunca**, porque se destruía su manejador antes de que Windows lo usara.
- **Reabrir desde la bandeja iniciaba una segunda copia** en lugar de restaurar la que estaba.
- **`WinDivert64.sys` seguía bloqueado tras cerrar**, porque abrir un manejador de controlador registra un servicio de núcleo que sigue en marcha.
- **Devolver el idioma a "Predeterminado del sistema" no hacía nada**, porque una única notificación de "todo ha cambiado" no se procesa con fiabilidad.
- **Desinstalar pedía reiniciar**, ya que la aplicación y su ayudante seguían reteniendo archivos.
- Las filas del registro tenían un relleno de elemento de lista que dejaba media línea en blanco entre entradas.

### Cambiado

- Las acciones del registro pasaron al panel de actividad de la derecha, en vez del final de la columna de configuración.

## [0.3.0] - 2026-09-09

### Añadido

- Registro de errores en `%LOCALAPPDATA%\LanBridge\logs\`, un archivo por proceso y ejecución, con las excepciones no controladas anotadas en lugar de terminar en silencio.
- Persistencia de ajustes en cada cambio, de modo que sobreviven a un fallo.
- Soporte del área de notificación con pregunta al cerrar.
- Interfaz en chino tradicional junto al inglés.
- Instalador MSI con accesos directos, metadatos de versión y entrada en Agregar o quitar programas.

### Corregido

- La compilación publicada arrancaba y moría dentro del entorno XAML, porque una aplicación WinUI sin empaquetar no lleva consigo su marcado compilado.

## [0.2.0] - 2026-09-09

### Corregido

- **La ventana no aparecía nunca tras la petición de elevación**, porque WinUI 3 no puede ejecutarse elevado; la interfaz corre sin elevación y cede el trabajo privilegiado a un ayudante que pide consentimiento una vez por sesión.

## [0.1.0] - 2026-09-09

### Añadido

- VPN por aplicación que rechaza la ruta predeterminada y el DNS empujados, de modo que solo la subred VPN cruza el túnel.
- Confinamiento opcional por proceso con WinDivert, descartando el tráfico hacia la subred VPN de cualquier proceso que no sea el objetivo.
- Relé de descubrimiento LAN para túneles que no transportan difusión, incluido el protocolo W3GS de Warcraft III, cuya información de partida solo se envía como respuesta unicast.
- Relé genérico de difusión UDP para otros juegos, configurado por puerto.
- Versión de línea de comandos del mismo motor, sin interfaz.

[0.5.32]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.32
[0.5.31]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.31
[0.5.30]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.30
[0.5.29]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.29
[0.5.28]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.28
[0.5.27]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.27
[0.5.26]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.26
[0.5.25]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.25
[0.5.24]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.24
[0.5.23]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.23
[0.5.22]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.22
[0.5.21]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.21
[0.5.20]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.20
[0.5.19]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.19
[0.5.18]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.18
[0.5.17]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.17
[0.5.16]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.16
[0.5.15]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.15
[0.5.14]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.14
[0.5.13]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.13
[0.5.12]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.12
[0.5.11]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.11
[0.5.10]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.10
[0.5.9]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.9
[0.5.8]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.8
[0.5.7]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.7
[0.5.6]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.6
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

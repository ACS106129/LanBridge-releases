# Registro de cambios

Aquí se recogen todos los cambios relevantes de LanBridge.

El formato sigue [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/) y el
versionado sigue [Versionado Semántico](https://semver.org/lang/es/).

## [0.5.25] - 2026-09-17

### Fixed

- **Los sitios solo pasaban por el túnel cuando ambos lados coincidían por casualidad en la
  dirección.** Una sesión real enrutó veintisiete nombres, puso ochenta rutas, confirmó cada
  una como en uso y movió la dirección de salida de Taiwán a Japón — y la aplicación siguió
  llegando a exactamente un destino por el túnel.

  Dieciocho de los veintisiete respondían distinto desde cada lado.
  apidgp-gameplayer.games.dmm.com daba una dirección de Taipéi preguntando desde aquí y una de
  Tokio preguntando por el túnel, y solo se enrutó la de Tokio. Pero la aplicación resuelve los
  nombres ella misma, desde aquí: le dijeron Taipéi y fue a Taipéi, una dirección que nadie
  había enrutado, por el adaptador de siempre, mientras cada ruta de la tabla seguía correcta y
  sin usar. El único destino que sí pasó era uno de los nueve cuyas respuestas coincidían.

  Ahora se enrutan las dos: la del túnel porque es la que debería usarse, y la local porque es
  la que se va a usar.

- **Los sitios se editan uno por fila, en una ventana propia.** Eran una sola caja donde pegar
  una lista, lo cual está bien con dos nombres y no con veintisiete: una errata se esconde y
  quitar el cuarto exige seleccionar exactamente ese trozo de texto. La página de ajustes ahora
  dice cuántos hay y nombra los primeros; la lista está detrás de un botón, con una fila por
  sitio y sitio para añadir otro.

- **Una descarga que el servidor corta se retoma donde quedó, en vez de tirarse.** Lo que pasó
  de verdad, según el informe: a los treinta minutos, con sesenta y tres de ciento cuatro
  megas descargados, el otro extremo colgó — "la respuesta terminó antes de tiempo, faltaban
  al menos 41252358 bytes". Ni el archivo ni la petición tenían nada malo; la conexión
  simplemente terminó, y todo lo ya descargado se tiró. Ahora cada parte se vuelve a pedir
  desde el byte al que llegó, hasta cinco veces, esperando un poco más entre intentos.
  Ahora nada termina una descarga salvo que usted la detenga. Ni un rechazo, ni un
  límite de peticiones, ni siquiera un "no encontrado": un archivo que se está reemplazando, o
  un nodo que aún no se ha puesto al día, responden así durante unos segundos, y no hay
  respuesta que un servidor pueda dar que valga menos que un intento más medio minuto después.

- **La 0.5.24 culpó de esto a un tiempo de espera, y se equivocaba.** Decía que un límite de
  quince minutos cortaba las descargas lentas cerca del final, y quitó el límite. Quitarlo no
  hace daño y el razonamiento sigue en pie, pero el fallo que se puso como prueba llevaba
  treinta minutos corriendo cuando ocurrió, así que un límite de quince no pudo ser lo que lo
  terminó. Estaba en el informe antes de publicar la afirmación, y no se leyó con cuidado.

## [0.5.24] - 2026-09-17

### Fixed

- **Una descarga de actualización lenta se tiraba justo antes de terminar.** El cliente daba
  quince minutos a la transferencia entera, y ese límite cubre leer el archivo, no solo
  llegar al servidor. Medido en una conexión real, el servidor de descargas daba unos
  0,10 MB/s, lo que pone un instalador de cien megas en más de dieciséis minutos: se
  descargaba casi entero y entonces fallaba. Ya no hay límite total. Cuánto esperar lo decide
  el usuario, y cancelar es cómo lo decide.

- **Las actualizaciones se descargan unas tres veces más rápido.** El límite resultó ser por
  conexión y no de la línea: una conexión daba 0,10 MB/s, mientras que cuatro conexiones
  trayendo partes distintas del mismo archivo a la vez sumaban 0,32 MB/s. Ahora el instalador
  se trae en cuatro partes a la vez — medido de principio a fin con un archivo de 122 MB a
  0,28 MB/s frente a 0,10, y comprobado byte a byte contra el archivo publicado, no solo por
  su tamaño.

  Solo cuando el servidor dice que sirve partes. Pedir un rango a uno que no lo hace se
  responde con el archivo entero, y cuatro archivos enteros escritos unos sobre otros dan un
  instalador corrupto del tamaño exacto.

- **El progreso se informa a un ritmo que la ventana puede usar.** Se enviaba cada 80 KB, que
  son mil trescientas actualizaciones para un instalador y cuatro veces eso con cuatro
  conexiones, cada una cruzando al hilo de la interfaz. Ahora cada 512 KB, y siempre una más
  al final para que la barra acabe donde acaba el archivo.

## [0.5.23] - 2026-09-16

### Added

- **La sesión deja escrito a dónde fue realmente el programa objetivo y qué parte de ello no
  pasó por el túnel.** Medir la dirección de salida dice si el túnel lleva lo que se enruta
  por él; no dice nada sobre si el tráfico que importa está enrutado siquiera — y enrutar una
  lista de nombres no sirve de nada para un nombre que nadie puso en ella. Ese hueco era todo
  el problema: la misma aplicación funciona con una VPN para toda la máquina y no con un
  puñado de rutas, y qué nombres van en ese puñado era una suposición que tiene que acertar o
  la función no hace nada.

  Así que ahora se observa. Cada destino que alcanza el objetivo se registra una vez, con la
  respuesta del propio sistema sobre si un paquete hacia allí sale por el túnel, y al final de
  la sesión se dice en qué quedó todo. Una sola ejecución nombra exactamente lo que falta.

- **Las direcciones se informan con el nombre al que responden.** En una red de distribución
  el nombre del nodo lleva su ubicación, y la ubicación es la pregunta: el mismo host respondió
  `…nrt57.r.cloudfront.net` a través de un túnel japonés y `…tpe53.r.cloudfront.net` desde
  Taipéi. Narita frente a Taipéi se ve de un vistazo en un nombre y no se ve en absoluto en una
  lista de direcciones.

### Fixed

- **Los nombres se siguen mientras dura la sesión, en vez de fijarse una sola vez al empezar.**
  Responden con un TTL de sesenta segundos. En una hora de observación no se movió ninguno, así
  que no era esto lo que fallaba — pero una sesión dura horas, un nombre puede moverse en
  cualquier minuto, y cuando lo hace la aplicación alcanza una dirección que nada enruta, el
  tráfico sale por donde siempre, y todas las rutas de la tabla siguen diciendo "en uso". El
  fallo tendría exactamente el aspecto del acierto.

- **La búsqueda de un resolutor que conteste se detiene en cuanto uno contesta.** Se hacía por
  nombre, y en un túnel real el primer candidato agotaba el tiempo por UDP y otra vez por TCP
  para cada nombre, seis segundos cada uno. Con cuatro nombres eran veinticuatro segundos antes
  de arrancar; con los veintinueve que una aplicación real resultó necesitar habrían sido casi
  tres minutos — y la conclusión fácil habría sido que la lista larga no era viable, en vez de
  que no lo era la búsqueda.

## [0.5.22] - 2026-09-16

### Added

- **La aplicación ahora mide, y deja escrito, si la dirección de salida cambió de verdad.**
  Mandar el tráfico de un programa por un túnel merece la pena por una sola razón: que el otro
  extremo lo vea llegar desde otro sitio. Todas las comprobaciones que hacía esta función hasta
  ahora eran un paso hacia eso y no eso — el comando que añadió la ruta, la ruta en la tabla con
  buena métrica, el nombre resuelto a través del túnel — y cada una de ellas ha sido cierta al
  menos una vez mientras el tráfico salía por el adaptador de siempre todo el rato.

  Así que ahora la misma pregunta se le hace a internet dos veces, una antes de tocar ninguna
  ruta y otra con todas ya puestas, y ambas respuestas van al registro. "No se pudo saber" se
  escribe tal cual, nunca como "no cambió".

### Fixed

- **El registro se dejaba justo la mitad de la sesión para la que haría falta.** Los pasos que
  muestra la ventana — qué dirección de túnel llegó, si arrancó el aislamiento por proceso, qué
  proceso se adoptó cuando el programa objetivo pasó el testigo a otro — iban a la ventana y a
  ningún otro sitio. Igual que todo lo que decía el filtro de paquetes y todo lo que decía el
  propio OpenVPN. Al releer un registro después quedaban las rutas y casi nada alrededor: ya han
  sido dos las veces que una pregunta sobre una sesión fallida no se ha podido responder desde
  él, incluida la de si el filtro llegó a arrancar. Ahora todo eso va al archivo.

- **La comprobación de que una ruta se está usando de verdad podía rechazar una que estaba a punto
  de funcionar.** La 0.5.21 empezó a verificar cada ruta en lugar de fiarse del código de salida del
  comando, y ese cambio era el correcto; solo que preguntaba en el mismo instante en que añadía la
  ruta. Una ruta que el sistema todavía no ha mirado es indistinguible de una que ha rechazado, así
  que un momento de retraso bastaba para tirar una ruta que iba a funcionar: la verificación
  derrotando aquello que verifica. Ahora se le conceden a la tabla de rutas unos cientos de
  milisegundos para asentarse.

- **El filtro de paquetes ahora deja constancia del filtro con el que se abrió.** Cuando el programa
  seguía saliendo a internet por IPv6 pese a tenerlo bloqueado, el registro no permitía distinguir si
  faltaba la cláusula o si simplemente nunca llegó a coincidir. Ahora sí.

## [0.5.21] - 2026-09-14

### Fixed

- **Los sitios que debían ir por la VPN no iban, y todo decía que sí.** 0.5.20 añadió las
  rutas, informó "ok" de cada una y las dejó en la tabla con buena métrica. Windows las
  ignoró todas.

  El siguiente salto estaba mal. Un túnel suele dar una dirección punto a punto —ésta era
  una /30, con sólo cuatro direcciones— y el salto se adivinó como el .1 de la red, que en
  un enlace así no existe. Windows no usa una ruta cuyo siguiente salto no alcanza, así que
  el tráfico salió por el adaptador de siempre. Y nada lo dijo: el comando que añade la ruta
  la aceptó y devolvió éxito.

  Ahora el siguiente salto se deduce de la dirección que el túnel recibió de verdad. Y
  "añadida" ya no significa "funcionando": tras cada ruta se pregunta al sistema por dónde
  enviaría realmente un paquete, y la que no resulta elegida se informa y se retira.

- **Un nombre que no pudo resolverse por el túnel se informaba como coincidente con la
  respuesta local.** No hay respuesta no es la misma respuesta.

- **La consulta por el túnel ahora recurre a TCP.** En el túnel donde se encontró esto, el
  resolutor no contestó nada por UDP mientras que la conexión al mismo puerto funcionaba.

## [0.5.20] - 2026-09-13

### Added

- **Sitios que puedes mandar por la VPN sin mandar la máquina entera.** Hasta ahora, un
  programa que necesitaba que un sitio lo *viera* llegar desde el otro extremo —en lugar de
  necesitar alcanzar una máquina allí— sólo tenía una opción: entregarlo todo.

  Nombra los sitios y sus direcciones se consultan y se enrutan por el túnel. Consultarlas
  por el túnel es lo que importa: una red de contenidos responde según de dónde vino la
  pregunta, y uno de estos nombres respondió con un nodo de Taipéi desde aquí y con
  direcciones distintas dos horas después.

  El registro dice lo que encontró para cada nombre, por ambos lados, coincidan o no.

  Dos cosas antes de activarlo: cambia la tabla de rutas de la máquina, así que viene vacío;
  y mientras hay sesión esos sitios sólo los alcanza la aplicación de destino. Al parar, se
  retira cada ruta añadida.

## [0.5.19] - 2026-09-13

### Added

- **Un sitio para el usuario y la contraseña.** Algunos servidores piden iniciar sesión y no
  había dónde decirlo. Un `auth-user-pass` sin archivo detrás significa "pregunta en la
  consola", y aquí openvpn arranca con su salida redirigida y sin ventana: hace una pregunta
  que nadie oye y luego informa de un inicio de sesión fallido. Gestionar perfiles tiene
  ahora un botón por perfil.

  La contraseña se guarda sin cifrar y el diálogo lo dice en lugar de insinuar lo contrario.
  Está en la carpeta de ese perfil, que sólo puedes abrir tú, SYSTEM y Administradores.
  Nunca se lee de vuelta para mostrarla.

### Fixed

- **La aplicación de destino salía a internet por IPv6, rodeando el túnel.** Encontrado
  observando una sesión real: cuatro minutos, cuatro destinos, y uno de ellos por IPv6. Esta
  máquina tiene dirección IPv6 global de su proveedor y el túnel es IPv4.

  Era una fuga en todos los modos, incluido el que entrega la máquina entera a la VPN. Ahora
  se descarta el IPv6 del destino mientras hay sesión; el de los demás no se toca.

- **"Sin novedades" sin haber preguntado.** Con el límite por hora agotado, la comprobación
  informaba de la versión que ya tenías como si hubiera mirado. Ahora dice que no pudo
  comprobar y cuándo lo reintentará, y conserva entre ejecuciones el validador que hace que
  comprobar no cueste nada.

## [0.5.18] - 2026-09-13

### Fixed

- **La ventana Acerca de agradecía a WinDivert sin decir bajo qué términos se usa.** Es la
  GNU LGPL v3, que pide al programa que la usa decirlo, nombrar la licencia y señalar la
  copia que distribuye; un agradecimiento no es ninguna de las tres. Ahora dice las tres.
  También se indica que OpenVPN se descarga de openvpn.net en vez de distribuirse aquí.
- **Las plazas libres de una sala de Warcraft III nunca cambiaban en la otra máquina.**
  Abre un puesto donde había una computadora y el otro seguía viendo la sala igual que
  antes, hasta que salía de la lista de partidas y volvía a entrar.

  Un par que ya tiene la sala en su lista no vuelve a leer el anuncio completo. Toma las
  cifras de un pequeño paquete que el anfitrión emite en difusión cada vez que la sala
  cambia, y sólo rehace la entrada cuando se reabre la lista. Ese paquete va por difusión,
  y la difusión es justo lo que aquí no se puede capturar: el juego ya ocupa el puerto en
  el que habría que escuchar. Así que ahora se deriva del anuncio y se envía cuando las
  cifras se mueven.

  Las cifras se comprueban antes de enviarse: se leen de una posición fija al final de un
  paquete cuya disposición se dedujo, y una partida tiene entre una y veinticuatro plazas y
  no puede tener más libres de las que tiene. Cualquier otra cosa significa que la lectura
  está mal, y entonces no se envía nada.

- **La descarga de la actualización seguía reteniendo la ventana.** 0.5.16 decía haberlo
  arreglado. La transferencia en segundo plano, la barra de progreso y el botón de cancelar
  estaban escritos y nada los llamaba nunca.

  Ahora ocurre de verdad en segundo plano, y al empezar la descarga el diálogo ofrece
  **Continuar en segundo plano**: la ventana se cierra, la transferencia sigue y se informa
  en la barra de la ventana principal, donde también puede cancelarse. Cancelar y fallar ya
  se distinguen — antes ambos abrían la página de la versión en el navegador.

## [0.5.17] - 2026-09-13

### Fixed

- **Que un jugador saliera de una sala de Warcraft III la cerraba para todos.** Se informó
  así: pon el puesto de alguien en computadora, abierto o cerrado y ya no puede volver a
  entrar. Esas son tres maneras de cortar su conexión, y que el jugador se marche por su
  cuenta hace lo mismo.

  Un socket a la escucha y cada conexión aceptada en él comparten un puerto local. El
  registro de qué puertos son del juego se llevaba por puerto, así que la escucha y las
  conexiones compartían una entrada, y la primera conexión en cerrarse se la llevaba.
  Warcraft sigue escuchando y sigue anunciándose, de modo que la sala permanece en la
  lista de todos, pero cada paquete que llega a ese puerto ya no es de nadie a ojos del
  filtro, y se descarta. Visible e inaccesible, para todos, hasta que el anfitrión crea
  otra partida.

  Ahora cada socket se registra por separado, y un puerto deja de ser del juego cuando se
  cierra el último, no el primero.

- **La aplicación de destino seguía ejecutándose como administrador.** 0.5.16 decía haberlo
  arreglado y no lo había hecho. Dar a un proceso la identidad del usuario conectado puede
  hacerse de dos formas, y piden permisos distintos: la que se usaba necesita un privilegio
  que un administrador con elevación no tiene ni puede obtener, así que fallaba siempre y
  el comportamiento anterior tomaba el relevo en silencio. Ahora usa aquella cuyo permiso
  sí posee.

## [0.5.16] - 2026-09-13

### Añadido

- **Un instalador en cada idioma que habla la aplicación.** Hablaba once y su instalador
  hablaba dos. Ahora hay once, cada uno con la página de códigos ANSI correcta.
- **La ventana se abre donde la dejaste.** Tamaño, posición y si estaba maximizada. Se
  guarda el tamaño restaurado, y una posición que ya no cae en ninguna pantalla se descarta.
- **Un icono nuevo.** El anterior era una barra con dos puntos y no decía nada sobre lo
  que hace esto. Ahora es una flecha que sale por la abertura de un anillo: el túnel, y la
  única aplicación que lo cruza. Dibujado por separado en cada tamaño. El anillo está
  abierto por donde sale la flecha, porque uno cerrado con una línea es la señal de
  prohibido.

### Corregido

- **Iniciar quedaba por debajo del pliegue.** Los dos botones eran lo último de la columna
  de tarjetas de configuración, y esa columna se desplaza. En cuanto las tarjetas bastaban
  para llenarla —y a una altura de ventana normal bastan— la acción principal de la
  aplicación era algo que había que buscar desplazándose. Ahora los botones están fijos
  bajo las tarjetas y son las tarjetas las que se desplazan detrás.
- **La aplicación destino se ejecutaba como administrador.** El ayudante que la inicia
  tiene que serlo, y un proceso hijo hereda el token de su padre. Un programa con
  privilegios está aislado del escritorio sin ellos: así es como un juego que inicia sesión
  por el navegador nunca recibe su código de autorización. Ahora se inicia con el token del
  shell, como tú.
- **Descargar una actualización bloqueaba toda la ventana.** Ahora ocurre en segundo plano,
  con el progreso en una barra de la ventana principal.

## [0.5.15] - 2026-09-13

### Corregido

- **Un solo rechazo del servidor terminaba el intento.** openvpn considera fatal un
  inicio de sesión rechazado y sale al primero, lo cual está bien para un servidor propio
  y mal para un repetidor público: esos rechazan porque están llenos o porque quien lo
  mantenía se fue, y el mismo perfil conecta un minuto después. Ahora reintenta, y para a
  los tres intentos para que una contraseña realmente equivocada se informe igualmente.
- **«EXITING auth-failure» no explicaba nada.** Suena a contraseña equivocada, y después
  de que un certificado ya fue aceptado normalmente no lo es. Ahora dice qué paso falló y
  qué significa.

## [0.5.14] - 2026-09-12

### Añadido

- **Un perfil importado ahora pertenece a la aplicación.** Antes se recordaba dónde estaba
  el archivo y se leía otra vez en cada arranque, lo cual funciona hasta que el archivo se
  mueve, se saca el pendrive o se vacía Descargas. Ahora se copia a una carpeta propia,
  junto con cada certificado y clave a los que se refiere, y esas referencias se reescriben
  para apuntar a las copias. Borra el original y no cambia nada.
- **Un sitio donde ver lo que se guarda.** Un botón Gestionar junto a Importar: qué hay,
  cuál se usa, renombrar, eliminar y una puerta a la carpeta.
- **OpenVPN, si no lo tienes.** Esta aplicación maneja el cliente comunitario de OpenVPN;
  no lo contiene. Antes, una máquina sin él recibía una frase pidiendo que lo instalaras.
  Ahora lo dice antes de empezar y ofrece descargar la versión actual del propio servidor
  de OpenVPN e instalarla, rechazando cualquier cosa que Windows no acepte o que no esté
  firmada por OpenVPN.
- **Pruebas para el instalador.** Lo que hay dentro del paquete y un recorrido por sus
  páginas en los dos idiomas. Se detiene en el resumen y cancela: pasar las pruebas no
  instala nada.
- **Una prueba que mira los píxeles.** Cada línea explicativa se fotografía en ambos temas
  y se mide contra lo que tiene detrás.

### Corregido

- **Tres líneas del cuadro Acerca de eran invisibles.** Estaban pintadas con un pincel
  tomado de los recursos de la aplicación, que se resuelve contra el tema de la propia
  aplicación — y una aplicación WinUI sin empaquetar no puede cambiarlo tras arrancar,
  mientras que los diálogos se dibujan con el tema que elegiste.
- **La comprobación de actualizaciones dejó de insistir.** Sesenta al día por hora es
  justo el límite sin autenticar; pasado eso todo se rechaza. Ahora lee cuándo vuelve el
  cupo y espera.

- **El instalador escribía encima de su propia ilustración.** Esos mapas de bits no son
  imágenes junto al texto: son el fondo sobre el que el diálogo escribe, en su propio color
  oscuro, y el diálogo decide dónde. Rellenar los 493 píxeles con un degradado azul dejaba
  cada título en oscuro sobre oscuro. Ahora la ilustración es una franja a la izquierda y un
  bloque a la derecha del banner, y el resto queda en blanco.
- **La actualización ofrecía a todos el instalador en inglés.** Una publicación lleva un
  MSI por idioma y el actualizador tomaba el primero de la lista, que es el que se subió
  antes. Ahora pide el que corresponde al idioma de la ventana, y recurre al inglés cuando
  ese idioma no tiene instalador propio.

### Cambiado

- Cada ajuste lleva debajo una línea que dice qué cambia y dónde se guarda.
- Acerca de explica cómo se buscan las actualizaciones y da crédito a OpenVPN y WinDivert.

## [0.5.13] - 2026-09-12

### Añadido

- **Sonido.** WinUI trae un sistema de sonido en cada control —foco, invocación, diálogos
  que se abren y cierran— y calla salvo que la aplicación lo pida. Esta nunca lo había
  pedido, así que cada pulsación ha sido silenciosa por omisión y no por decisión. Ahora es
  una decisión, es espacial, y hay una casilla en la configuración para quien prefiera que
  una utilidad se calle.
- **Movimiento donde ha pasado algo.** Las dos columnas aparecen mientras la ventana se
  arma, el texto de estado sube al cambiar, el recuento retransmitido da un salto al subir,
  la luz de estado respira mientras hay sesión, y una línea de aviso empuja a sus vecinos
  en vez de surgir de la nada.
- **El dibujo informa en lugar de actuar.** Antes hacía la misma animación pasara o no
  algo, que es decoración vestida de instrumento. Ahora está atenuado y quieto sin sesión,
  se mueve con ella, y la celda bloqueada muestra los paquetes que el guardián ha
  descartado de verdad —un número que la interfaz nunca había recibido, porque nadie se
  había suscrito al evento que lo lleva.
- **Un instalador que se parece a este producto**, con imágenes generadas en vez de las de
  relleno de WiX.
- **Un instalador en tu idioma**: un MSI por idioma en vez de inglés para todos. Inglés y
  chino tradicional para empezar.

### Cambiado

- El aviso del confinamiento por proceso aparece ahora cuando la casilla está **vacía**,
  que es el estado que merece advertencia, y dice qué significa ese estado en vez de
  repetir la etiqueta. Los dos avisos llevan además marcas distintas: un globo para adónde
  va el tráfico, un candado abierto para quién puede usarlo.

## [0.5.12] - 2026-09-12

### Corregido

- **La carpeta de instalación contenía ochenta y ocho carpetas de traducciones de idiomas
  que esta aplicación no ofrece** —af-ZA, sl-SI, fil-PH y demás—. Son las cadenas del
  propio Windows App SDK, distribuidas como recursos Win32 y no como ensamblados satélite
  de .NET, así que el ajuste habitual para recortarlas no les llega. Ahora solo quedan las
  que corresponden a un idioma que la interfaz habla: quince en lugar de ochenta y ocho.

### Cambiado

- La opción por proceso se llama *Solo la aplicación objetivo puede hablar con la VPN*,
  que es lo que hace. Nunca gobernó el túnel entero.

### Añadido

- **Diez comprobaciones más que manejan la ventana real**, sobre los diálogos y lo que
  contienen. Escribirlas encontró dos cosas por su cuenta: un diálogo aquí no es una
  ventana ni se llama como parecía, de modo que cuatro pruebas buscaban algo que nunca ha
  existido; e Iniciar está deshabilitado sin un perfil en lugar de aceptar la pulsación y
  no hacer nada.

## [0.5.11] - 2026-09-12

### Corregido

- **El modo oscuro era texto blanco sobre página blanca.** El intento anterior pintaba el
  fondo en un elemento y aplicaba el tema al de dentro, así que el texto se resolvía en el
  tema oscuro y la superficie tras él en el claro. El tema va ahora en el elemento que
  pinta el fondo, donde ambos coinciden.
- **El icono de la sección del objetivo se dibujaba como un cuadro vacío.** Que un punto de
  código esté en el mapa de caracteres de una fuente no significa que la fuente tenga un
  glifo para él. Las tres marcas de sección son emoji ahora.
- **Los controles de cada sección aparecían centrados** en una tarjeta que ya tenía la
  anchura correcta. Un expansor se estira a sí mismo, pero no a su contenido.
- **Elegir adjuntarse y pulsar Iniciar sin escoger un programa lanzaba una excepción.** La
  comprobación añadida para un ejecutable ausente no cubre el modo de adjuntarse, al que le
  falta otra cosa. Ahora dice qué falta, y el mensaje ya no pide teclear un identificador
  de proceso en un control que es una lista.

### Añadido

- **Pruebas que manejan la ventana real.** Todos los defectos visuales notificados hasta
  ahora pasarían cualquier prueba unitaria del proyecto, porque ninguno trata de lo que
  devuelve un método. Diecinueve comprobaciones abren ahora la compilación publicada y
  miran.

## [0.5.10] - 2026-09-12

### Corregido

- **La línea de advertencia ensanchaba la columna en vez de ajustarse.** Una pila
  horizontal mide a sus hijos con anchura ilimitada, así que un bloque de texto ajustable
  dentro de ella nunca se ajusta: ensancha todo lo que tiene al lado, incluido el botón de
  arriba. Ambas notas están ahora en una cuadrícula que da al texto una anchura real.
- **La configuración y la información de la aplicación aparecían dos veces.** El par que
  estaba junto a las tarjetas de estado se quedó ahí al moverlas arriba a la derecha.
- **El selector de procesos ofrecía esta misma aplicación.** Adjuntar el túnel a la ventana
  que lo configura no es algo que nadie pretenda.

### Cambiado

- **El dibujo ocupa el espacio que se le dio.** Las rutas se estiran con la ventana en vez
  de quedarse con una anchura fija en la esquina de una tarjeta grande y vacía, y los
  paquetes recorren esa anchura entera.
- **La nota del confinamiento por proceso solo aparece cuando está activo**, con la misma
  marca de advertencia que la de enrutado, y dice lo que hace: se detienen los paquetes de
  todos los demás programas de aquí.
- La ruta del programa objetivo se ve completa al pasar el ratón, por estrecha que sea la
  caja.

## [0.5.9] - 2026-09-12

### Corregido

- **El modo oscuro era inservible.** La página nunca pintaba un fondo propio, así que el
  texto seguía al tema y la superficie de detrás no: texto claro sobre fondo claro. Los
  cuadros de diálogo también conservaban la apariencia del sistema, porque un diálogo lo
  aloja la raíz de la ventana y no el elemento al que se aplicó el tema, y había que
  indicárselo aparte.

### Añadido

- **Los dos interruptores de confinamiento son ahora un dibujo.** Una cuadrícula de quién
  envía frente a hacia dónde, con tráfico recorriendo cada ruta: por el túnel, por la
  salida de siempre, o detenido. Las cuatro celdas son todas las combinaciones de los dos
  ajustes, y responden de un vistazo lo que dos párrafos de texto no conseguían.
- **Adjuntarse elige de una lista de programas en ejecución** en vez de pedir un
  identificador de proceso buscado en otra parte, con un botón para actualizarla.

### Cambiado

- El aviso de enrutado solo aparece cuando esa opción está activada, dice una sola cosa y
  la dice en color de advertencia.
- Las secciones perdieron la numeración; nunca fueron pasos a seguir en orden.
- La configuración y la información de la aplicación se movieron arriba a la derecha, con
  las tarjetas de estado justo debajo.

## [0.5.8] - 2026-09-12

### Corregido

- **Limitar el túnel a una sola aplicación solo funcionaba en un sentido.** Descartaba el
  tráfico que otros programas de aquí enviaban a la VPN, y no hacía nada con el que llegaba
  desde ella, así que todos los demás programas de este equipo seguían siendo alcanzables
  desde el otro extremo — que es la mitad que importa cuando no sabes quién hay allí. Ahora
  se aplica en ambos sentidos, y la explicación lo dice en vez de prometer más de lo que
  hacía.

### Cambiado

- **La opción de enrutado está ahora al revés.** Mantener el resto del equipo fuera del
  túnel es el estado seguro y lo que casi todo el mundo quiere, así que no debería haber
  que activarlo. La casilla dice ahora *Enviar todo el tráfico por la VPN*, está desactivada
  de forma predeterminada y explica qué implica activarla: todo lo que envía este equipo
  pasa primero por el servidor VPN, así que quien lo administre lo ve todo. Eso importa
  sobre todo con un perfil que te haya dado otra persona.

### Añadido

- **Apariencia clara y oscura**, o seguir al sistema, aplicada al instante y recordada. En
  Apariencia, dentro de la configuración.
- **Iconos por toda la interfaz** —en cada sección, en Iniciar y Detener y en las acciones
  del registro— y una luz de estado que está verde mientras hay una sesión en marcha.

## [0.5.7] - 2026-09-12

### Corregido

- **No se podía cancelar una descarga.** El botón decía *Cancelar* y no se podía pulsar: la
  descarga se ejecutaba reteniendo el aplazamiento del clic del cuadro de diálogo, y un
  diálogo con un aplazamiento pendiente desactiva sus propios botones, incluido el único que
  habría podido detenerla. Ahora la transferencia corre junto al diálogo en vez de dentro de
  su controlador de clic, así que el botón está activo exactamente mientras haya algo que
  cancelar.
- **Una descarga cancelada o fallida dejaba su archivo incompleto**, uno por intento, para
  siempre. Ahora el archivo incompleto se descarta cuando la transferencia no termina, y una
  descarga completada limpia los instaladores anteriores.
- **Pulsar Iniciar sin nada que iniciar no hacía absolutamente nada**: ni mensaje, ni
  registro, ni cambio. Sin perfil, o sin aplicación elegida, ahora dice cuál falta en vez de
  parecer averiado.

### Cambiado

- **Instalar una actualización con una sesión en marcha avisa primero**, y la respuesta
  segura es la predeterminada. Instalar detiene el túnel y desconecta la aplicación
  objetivo, que no es algo que deba descubrirse después.

## [0.5.6] - 2026-09-12

### Corregido

- **Una versión nueva se detecta ahora en torno a un minuto después de publicarse**, en vez
  de en la siguiente comprobación programada. Preguntar tan a menudo sale gratis porque la
  petición es condicional: se devuelve el validador de la respuesta anterior y, mientras la
  versión no cambie, la respuesta es "sin modificar" —sin cuerpo y sin contar para el límite
  de peticiones—. Solo una versión realmente nueva consume una petición. Esto sigue siendo
  consultar y no recibir aviso, así que es un minuto y no un instante, pero no hay que
  pulsar nada ni reiniciar nada.
- **Descartar el aviso de actualización no dejaba forma de volver a él.** Cerrarlo era
  definitivo durante la sesión y solo se recuperaba reiniciando. Ahora tanto el cuadro de
  información como la configuración ofrecen *Actualizar ahora* mientras haya una esperando,
  así que descartar el aviso descarta solo el aviso.
- **El botón decía *Detener* cuando ya no quedaba nada que detener.** Al salir la aplicación
  objetivo, la sesión espera hasta veinte segundos por si un lanzador cede el paso a otro
  proceso: durante ese rato, aquello para lo que existe la sesión ya está muerto. En esa
  ventana el botón dice *Forzar detención*, que es lo que hace al pulsarlo: terminar la
  sesión ya en lugar de esperar el relevo.

### Cambiado

- **Todo lo relativo a las actualizaciones está ahora en el cuadro de información de la
  aplicación**, y su botón lleva un distintivo mientras hay una esperando. La comprobación
  automática, comprobar ahora, cuándo se comprobó por última vez y la propia actualización
  conviven con la versión con la que se comparan, en lugar de estar repartidos entre ahí y
  la configuración.
- **El instalador descargado se conserva si eliges *Más tarde*.** Antes, no instalar en el
  momento tiraba la descarga; ahora el mismo botón ofrece *Instalar ahora* hasta que la
  versión a la que pertenece quede superada.

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

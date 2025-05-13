# Examen_Redes2


# Parte I – Misiones de Conocimiento Teórico

## Misión 1: Reconexión en la Base Eco (Hoth) – Direccionamiento IP y Subredes
Situación: La Base Eco en Hoth ha quedado incomunicada tras un bombardeo imperial. La General Leia Organa te encomienda diseñar un nuevo esquema de red para restablecer la comunicación interna y con la flota rebelde. Hoth alberga varios departamentos (mando, defensa, médico, hangar) y cada uno necesita su propia subred IP debido a requisitos de seguridad y cantidad de dispositivos. Disponéis de un bloque de direcciones limitado y es crucial usarlo eficientemente.

Narrativa: Bajo la tormenta helada de Hoth, los generadores de energía zumban mientras instalas nuevos equipos de red. Leia, con su atuendo invernal, te explica: "Necesitamos reorganizar las comunicaciones de la base. Cada sector debe tener su red, y todas deben poder conectarse con la antena principal. El rango de direcciones del que disponemos es limitado, joven padawan. Debes dividir la red sabiamente, como un Jedi dividiría sus esfuerzos.".

Objetivo de la misión: Diseña un plan de direccionamiento IP para la Base Eco utilizando subnetting. Parte de una red asignada (por ejemplo, 172.16.0.0/24) y subdivide en subredes adecuadas para cada departamento, cumpliendo estos requisitos (ficticios):

- Comando Central: ~50 hosts (dispositivos)

- Defensa Perimetral: ~30 hosts

- Centro Médico: ~20 hosts

- Hangar y Taller: ~14 hosts

Además, reserva una subred adicional para el enlace troncal con la antena de comunicaciones que conecta Hoth con el resto de la galaxia. Debes detallar la dirección de red, máscara y rango de hosts de cada subred resultante.

Pregunta: ¿Cómo dividirías la red 172.16.0.0/24 en subredes para satisfacer las necesidades anteriores, asignando direcciones IP a cada segmento de la base? Indica las subredes obtenidas (con su notación de máscara /xx), la cantidad de hosts útiles en cada una, y especifica qué subred se destinaría al enlace troncal interplanetario.


## Respuesta
Para dividir nuestra red en las subredes necesarias con subneting necesitamos dividir esas direcciónes utiles 254 hosts. No se pueden crear cinco subredes igualesdado que no hay suficientes hosts asi que para ahorrar en lo mas posible en este planeta inospito reducimos el tamaño al minimo siempre y cuando aun se cumpla los requisitos.
- **Red** 176.16.0.0/24
- **Total de direcciones disponibles:** 256  
- **Total de direcciones útiles:** 254

Para hacer eso modificaremos la netmask de cada subred dependiendo del numero de hosts que necesiten:
- **Comando central:** Se necesitan 50, siendo el numero mas proximo que se puede conseguir con una netmask es 64 cojemos la red y le ponemos una mascara de /26 para dejar solo 64 host en la subred y solo 62 utiles.
- **Defensa perimetral:** En este caso son 30 hosts los necesarios, dejando 32 bits de dirreccionamiento con una mascara de /27 nos deja los 30 hosts necesarios para la subred
- **Centro medico:** Con 20 host necesitados para poder atender a nuestros enfermos tras el bombardeo se utiliza una mascara de /27 tambien porque aunque nos deja host extras un mascara mas grande nos dejaria con un numero insufciente de direcciones.
- **Hangar y taller:** Con solo 14 host una mascara de /28 se nos quedarian los 14 host necesarios para poder conectar todos nuestros dispositivos.
- **Red troncal:** Para la red troncal si no es muy grande una mascara de /29 bastaría y ademas permitiria una cierta cantidad de flexibilidad dadas los 6 hosts utiles

| Departamento           | Subred              | Máscara             | Default gateway | Hosts útiles | Rango de Hosts               | Dirección de Broadcast |
|------------------------|---------------------|----------------------|----------------|--------------|------------------------------|-------------------------|
| Comando Central        | 172.16.0.0/26       | 255.255.255.192      | 172.16.0.1  |62           | 172.16.0.2 – 172.16.0.62     | 172.16.0.63             |
| Defensa Perimetral     | 172.16.0.64/27      | 255.255.255.224      | 172.16.0.65 |30           | 172.16.0.66 – 172.16.0.94    | 172.16.0.95             |
| Centro Médico          | 172.16.0.96/27      | 255.255.255.224      | 172.16.0.97 |30           | 172.16.0.98 – 172.16.0.126   | 172.16.0.127            |
| Hangar y Taller        | 172.16.0.128/28     | 255.255.255.240      | 172.16.0.129 |14           | 172.16.0.128 – 172.16.0.142  | 172.16.0.143            |
| Enlace Troncal (Antena) | 172.16.0.144/29    | 255.255.255.248      | 172.16.0.145 |  6        | 172.16.0.146 – 172.16.0.150 |  172.16.0.151 |

Este esque permite una cierta escalabilidad dado que no tadas las dirrecciones ip estan ocupadas, aunque seria complicado porque para poder modificar cualquier red excepto la troncal tienes que modificar todas las superiores.

## Misión 2: Sabiduría de Yoda – Algoritmos de Enrutamiento y Rutas
Situación: Completando tu entrenamiento en Dagobah, el Maestro Yoda evalúa tu comprensión de cómo circula la información por la galaxia. Las rutas que toman los paquetes a través de los nodos de la HoloRed pueden establecerse de distintas formas: estáticas o dinámicas, simples o inteligentes. Yoda quiere saber si distingues la filosofía detrás de los algoritmos de enrutamiento clave.

Narrativa: En la neblina de los pantanos de Dagobah, Yoda cierra los ojos y enuncia en su peculiar forma: "En tus redes, joven padawan, dos caminos existen para los datos: uno fijo y otro en movimiento constante. El primero, estático es – manual, control total brinda; el segundo, dinámico – se adapta y vive la red. Grandes diferencias hay... ¿Comprenderlas tú las puedes?".

A su lado, luces parpadeantes de un viejo router imperial recuperado iluminan tu rostro. Debes explicarle a Yoda las diferencias que percibes en la Fuerza… de la red.

Pregunta: Compara el enrutamiento estático con el enrutamiento dinámico. ¿Cuáles son las ventajas e inconvenientes de cada enfoque en la administración de rutas? En tu respuesta, menciona al menos un protocolo de enrutamiento dinámico (por ejemplo, RIP u OSPF) y comenta por qué los protocolos de vector de distancia difieren de los de estado de enlace en términos de rendimiento y complejidad​

## Respuesta
A traves de la xonexion de la fuerza en ese viejo router se sienten dos tipos de rutado a traves de la holo-net. Uno vivo y uno quieto.

- **El quieto o rutado estatico** es un tipo de rutado completamente manual y no automatico, el controlador galactico tiene que establecer cada ruta a mano con cada red mascara y puerto, es facil de implementar en redes pequeñas y mas seguro ya que es una ruta estatica que no se puede manipular y ademas aunque pesado y costoso en tiempo es facil de manipular y controlar ya que el administrador decide todo la red. Pero tal rutado no viene sin sus desventajas, es debil contra cambios ya que si el camino cae la ruta se corta a no ser que alguien manualmente la manipule, si una red cambia tiene que ir un operario del cuerpo de ingenieros galacticos a cambiar las rutas y es muy dificil y costoso de implementar en una gran red como una planetaria

- **El vivo o rutado dinamico** es uno que no depende de su controlador, una vez se haya inciado la configuracion primaria estos routers se adaptaran a los cambios, hace que expandir la red sea mas facil ya que no tienes que establecer cada ruta manualmente y si una conexión cae no se aislara y buscara rutas alternativas. Pero como todo en esta galaxia nada es perfecto, este tipo tambien tiene sus desventajas: es mas complejo que el estático y necesita de un conocimiento mas profundo, consume mas recursos que el estatico también y necesita una configuracion mas precisa dado que un error podria dar resultado a paquetes perdidos o bucles.

| Característica           | Vector de Distancia (RIP)         | Estado de Enlace (OSPF)              |
|--------------------------|-----------------------------------|--------------------------------------|
| **Vista de la red**      | Desde el punto de vista del vecino | Vista completa de la topología       |
| **Métrica usada**        | Saltos                             | Costo (ancho de banda)               |
| **Convergencia**         | Lenta                              | Rápida                               |
| **Escalabilidad**        | Limitada                           | Alta                                 |
| **Complejidad**          | Baja                               | Alta                                 |

### Ejemplos de routing dinamico
#### RIP (Routing Information Protocol)
- **Tipo:** Vector de Distancia
- **Métrica:** Número de saltos (máximo 15)
- **Ventajas:** Muy sencillo de configurar
- **Desventajas:** Lento en converger, poco eficiente en redes grandes

#### OSPF (Open Shortest Path First)
- **Tipo:** Estado de Enlace
- **Métrica:** Costo (basado en ancho de banda)
- **Ventajas:** Convergencia rápida, diseño jerárquico, apto para grandes redes
- **Desventajas:** Más complejo de configurar y mantener

| Tipo de Enrutamiento | ✅ Ventajas                                                                 | ❌ Desventajas                                                                 |
|----------------------|------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| **Estático**         | - Control total sobre las rutas                                             | - No se adapta automáticamente a cambios                                       |
|                      | - Menor consumo de recursos del router                                      | - Requiere mantenimiento manual constante                                      |
|                      | - Mayor seguridad (sin compartir información con otros routers)             | - No escalable en redes grandes                                                |
|                      | - Sencillez en redes pequeñas o estables                                    | - Alta probabilidad de errores humanos                                         |
|                      |                                                                              | - Difícil implementar redundancia automática                                   |
|                      |                                                                              |                                                                                |
| **Dinámico**         | - Se adapta automáticamente a cambios en la red                             | - Mayor complejidad en la configuración inicial                               |
|                      | - Requiere menos intervención manual a largo plazo                          | - Consumo de CPU, memoria y ancho de banda                                     |
|                      | - Escalable y más eficiente en redes grandes                                | - Puede ser menos seguro si no se configura correctamente                     |
|                      | - Permite redundancia y rutas alternativas automáticamente                  | - Dependencia del protocolo utilizado (RIP, OSPF, etc.)                        |
|                      | - Convergencia automática ante fallos                                       | - Posibles errores de enrutamiento si hay mala planificación                  |


## Misión 3: Los Nombres del Holonet – DNS y Resolución de Nombres
Situación: La flota rebelde ha interceptado transmisiones imperiales que mencionan distintos códigos de planeta y nombres en clave. Para coordinar un contraataque, la Alianza necesita entender cómo los nombres de dominio galácticos se traducen en localizaciones reales. En otras palabras, requieren restablecer un servicio DNS rebelde que asocie nombres de sistemas estelares con direcciones de la red. Sin DNS, comunicarse es tan difícil como encontrar un Ewok en la noche de Endor.

Narrativa: A bordo de la nave de mando Home One, los técnicos rebelde te muestran un panel donde parpadea la petición “Unknown host”. El Almirante Ackbar frunce el ceño mientras exclaims: "¡Es una trampa... de nombres! Nuestros sistemas no reconocen las direcciones de destino." Mon Mothma asiente con gravedad: "Debemos reconstruir nuestro directorio de comunicaciones. Aprendiz, ¿cómo funciona este Sistema de Nombres de Dominio nuestro? ¿Por qué es tan importante?".

Tú recuerdas tus lecciones sobre cómo en la Red (o la HoloRed) se gestionan las direcciones simbólicas. Un nombre como “echo.base” debe traducirse a una dirección IP para establecer conexión. Ha llegado el momento de explicarlo claramente.

Pregunta: Explica el funcionamiento básico del sistema DNS y su importancia en la comunicación en redes. ¿Cómo realiza la red rebelde (o cualquier red TCP/IP) la resolución de nombres de dominio a direcciones IP? Incluye en tu explicación qué es un servidor DNS y un registro (por ejemplo, un registro A), ilustrando con un ejemplo simple (por ejemplo: traducir holonet.rebelion.org a una dirección IP)​

es.wikipedia.org
. Además, menciona brevemente qué sucede si el servidor DNS no está disponible y cómo eso afectaría a las comunicaciones de la Alianza.

## Respuesta

La función de un DNS es traducir nombres de dominio legibles, como holonet.rebelion.org, en direcciones IP numéricas que los sistemas de red pueden procesar, por ejemplo, 203.0.113.42. Sin esta traducción, los dispositivos no podrían establecer comunicación usando nombres simbólicos y la red rebelde colapsaría dado que los dispositivos de red no sabrian llegar a ningun sitio.  
Solo se podria llegar si se tiene la IP numerica para cada servicio y recordar eso es practicamente imposible, esto es un gran problema para la flota rebelde.

### ¿Cómo funciona la resolución de nombres?

Cuando un droide o terminal rebelde quiere acceder a `holonet.rebelion.org`, esto es lo que ocurre:

1. **Consulta local**: El sistema revisa si ya conoce la IP en su caché DNS.
2. **Petición al servidor DNS**: Si no la encuentra, pregunta a su **servidor DNS** configurado (puede ser un servidor rebelde o uno público).
3. **Búsqueda jerárquica** Si el DNS no la encuentra:
   - Se contacta primero al **servidor raíz (.)**
   - Luego al **servidor TLD** (`.org`)
   - Y finalmente al **servidor autoritativo de `rebelion.org`**
4. **Respuesta final**: El servidor devuelve un **registro A**, que asocia el nombre con su IP.


## Misión 4: “Es una trampa… de protocolos!” – TCP vs UDP en las transmisiones
Situación: Durante la batalla espacial sobre Endor, los ingenieros de comunicación rebelde notan comportamientos distintos en las transmisiones de datos. Algunas comunicaciones deben ser rápidas aunque ocasionalmente se pierda información (por ejemplo, un stream de vídeo de una cámara X-Wing), mientras que otras deben llegar íntegras y en orden aunque tarden un poco más (por ejemplo, la transferencia de los planos de la Estrella de la Muerte). Estas diferencias corresponden al uso de distintos protocolos de transporte: UDP y TCP. Luke Skywalker, ahora piloteando su X-Wing y ejerciendo de líder en el ataque, te pregunta por qué percibe lagos de datos en unas transmisiones y retrasos en otras.

Narrativa: En medio del fragor de la batalla, ves cómo R2-D2 proyecta diagramas de paquetes dentro del X-Wing de Luke. "Algunas de estas tramas van rápidas como el Halcón Milenario, pero otras llegan seguras como Yoda al Consejo," comenta Luke por el comunicador, intentando comprender. Tú, desde la sala de control, le explicas que siente la diferencia entre los dos grandes protocolos de la capa de transporte.

Pregunta: Compara los protocolos TCP y UDP y sus características en contexto de la transmisión de datos. ¿Por qué TCP se considera un protocolo confiable y orientado a conexión, y qué implica eso en cuanto a rendimiento? ¿Por qué UDP es no confiable y sin conexión, y en qué casos su rapidez resulta ventajosa?​

es.linkedin.com
​
es.linkedin.com
. En tu respuesta, menciona ejemplos de aplicaciones o situaciones galácticas para cada protocolo: por ejemplo, qué tipo de datos enviarías mediante UDP durante una misión crítica, y cuál vía TCP en comunicaciones rutinarias. (Pista: TCP garantiza la entrega de datos completa y ordenada – ideal para transmitir planes estratégicos; UDP minimiza retrasos – útil para enviar coordenadas de combate en tiempo real, aunque alguna pueda perderse.).

## Respuesta
En medio del asalto a la nueva Estrella de la Muerte, los ingenieros de la Alianza Rebelde detectan distintos comportamientos en las transmisiones. Algunas  fluyen velozmente, aunque con pérdidas ocasionales. Otras  llegan completas pero con cierto retraso. El joven Skywalker nota esto y pregunta: "¿Por qué algunas señales llegan como cohetes, y otras como si atravesaran un campo de asteroides?"

La respuesta está en la **capa de transporte** del modelo TCP/IP, donde operan dos grandes protocolos: **TCP** y **UDP**. Cada uno cumple una función estratégica distinta, como un Jedi y un contrabandista con estilos opuestos pero igual de útiles.

###  ¿Por qué TCP se considera confiable y orientado a conexión?

TCP (Transmission Control Protocol) establece una **"conexión" previa entre emisor y receptor**, como si ambos se dieran un apretón de manos antes de hablar. Esto implica que cada segmento de datos enviado requiere confirmación (ACK), y si no llega, se retransmite. Además, TCP ofrece características esenciales como el control de flujo, confirmación de entrega, corrección de errores y retransmisión de paquetes perdidos, garantizando así la integridad de los archivos transferidos. Aunque introduce una ligera latencia, esta es aceptable en aplicaciones donde la precisión y la fiabilidad son prioritarias.

Todo esto lo convierte en un protocolo **lento pero seguro**, ideal cuando **la integridad de la información es prioritaria**, aunque eso implique cierta latencia.

#### Ejemplos galácticos de uso de TCP:
- Transferencia de los **planos de la Estrella de la Muerte**
- Descarga de informes estratégicos desde la base Echo
- Comunicaciones cifradas entre flotas en modo seguro


### ¿Por qué UDP se considera rápido y no confiable?

UDP (User Datagram Protocol) **no establece conexión**: simplemente lanza los paquetes sin asegurarse de que lleguen ni en qué orden. Esto reduce mucho la latencia y el uso de recursos, a costa de fiabilidad. No requiere confirmaciones ni mantiene el estado de la conexión, lo que reduce significativamente la latencia. Aunque puede haber pequeñas pérdidas de información, estas no afectan de forma significativa la calidad general del servicio, priorizando así la continuidad y la velocidad del flujo de datos.

UDP es perfecto para **situaciones donde la velocidad es más importante que la precisión**, y donde perder uno o dos paquetes no compromete la misión.

#### Ejemplos galácticos de uso de UDP:
- Transmisión de **video en vivo desde una cámara en un X-Wing**
- Envío de **coordenadas de combate** en tiempo real
- Comunicación de sensores y telemetría de naves durante maniobras evasivas



## Misión 5: Comunicación Segura o lado oscuro – Criptografía y Seguridad de la Red
Situación: La victoria rebelde depende de que sus comunicaciones permanezcan secretas. Se rumorea que espías imperiales (e incluso usuarios del lado oscuro) intentan interceptar los mensajes de la Alianza. La líder Mon Mothma te encomienda diseñar un protocolo de comunicación cifrado para que los mensajes entre bases rebeldes no puedan ser leídos por el Imperio aunque sean capturados.

Narrativa: En la sala de guerra de la base rebelde, Mon Mothma se dirige a ti con semblante serio: "Hemos logrado infiltrar agentes en Coruscant, pero comunicarse con ellos es arriesgado. El Imperio intercepta cualquier señal. Aprendiz, necesitamos que nuestras transmisiones sean indescifrables para el enemigo." A tu lado, C-3PO comenta nerviosamente en más de seis millones de formas de comunicación: "Si caemos en manos imperiales, prefiero que apaguen mis circuitos a revelar nuestros mensajes."

Recuerdas que cifrar es esencial: usar claves secretas para que sólo los rebeldes autorizados puedan leer un mensaje. También piensas en las dos formas en que esto se puede lograr: con clave simétrica (una misma clave secreta compartida) o con claves públicas/privadas (criptografía asimétrica). Es hora de demostrar tu conocimiento en seguridad.

Pregunta: Explica brevemente la diferencia entre cifrado simétrico y cifrado asimétrico en el contexto de las comunicaciones de la Alianza. ¿Cómo funciona cada esquema y qué ventajas ofrece?​

blog.mailfence.com
Por ejemplo, si Leia y Luke comparten una frase clave secreta para cifrar sus holomensajes, ¿qué tipo de cifrado es ese? En cambio, si la Alianza quiere enviar un mensaje a un nuevo aliado sin haber intercambiado una clave secreta previamente, ¿cómo podría ayudar el cifrado asimétrico (claves pública/privada) en ese caso? Comenta también sobre la importancia de autenticación y no repudio en los mensajes (cómo podemos estar seguros de que el mensaje no ha sido alterado y proviene realmente de quien dice ser). Finalmente, menciona por qué utilizar un protocolo seguro (como SSH en lugar de Telnet) es crucial al administrar remotamente los equipos de la red rebelde​
hackernoon.com

## Respuesta

La victoria de la Alianza Rebelde depende de mantener sus comunicaciones en secreto. Espías imperiales y usuarios del lado oscuro intentan interceptar los mensajes de la Alianza. Mon Mothma te encomienda diseñar un protocolo de comunicación cifrado para que los mensajes entre bases rebeldes no puedan ser leídos por el Imperio, incluso si son capturados.

### Cifrado Simétrico vs. Asimétrico

#### Cifrado Simétrico

En el cifrado simétrico, se utiliza la misma clave secreta tanto para cifrar como para descifrar la información. Es fundamental que todos los usuarios que quieran cifrar o descifrar el mensaje tengan esta clave secreta; de lo contrario, no podrán hacerlo. Este método es eficiente y rápido, pero presenta desafíos en la distribución segura de la clave.

#### Cifrado Asimétrico

El cifrado asimétrico utiliza un par de claves: una pública y otra privada. La clave pública se distribuye abiertamente, mientras que la clave privada se mantiene en secreto. Cualquiera puede cifrar un mensaje utilizando la clave pública del destinatario, pero solo el destinatario puede descifrarlo con su clave privada. Este método resuelve el problema de la distribución segura de claves.

| Característica              | 🔑 Cifrado Simétrico                                 | 🔐 Cifrado Asimétrico                                  |
|----------------------------|------------------------------------------------------|--------------------------------------------------------|
| Número de claves           | 1 (misma clave para cifrar y descifrar)              | 2 (clave pública y clave privada)                      |
| Intercambio de clave       | Requiere distribución segura de la clave secreta     | No necesita intercambiar claves privadas               |
| Velocidad de operación     | Alta (rápido y eficiente)                            | Más lento (cálculos criptográficos más complejos)      |
| Complejidad                | Menor                                                | Mayor                                                  |
| Seguridad del intercambio  | Más vulnerable si la clave es interceptada           | Más seguro para comunicación inicial con desconocidos  |
| Uso típico                 | Grandes volúmenes de datos, canales ya seguros       | Establecer comunicación segura sin clave previa        |
| Ejemplo rebelde            | Leia y Luke comparten una frase clave secreta        | La Alianza envía un mensaje cifrado a un nuevo espía   |
| Aplicación real            | AES, DES                                             | RSA, ECC                                               |


### Autenticación y No Repudio
La autenticación asegura que el mensaje proviene realmente de quien dice ser, mientras que el no repudio garantiza que el emisor no pueda negar haber enviado el mensaje. En el contexto de la Alianza, esto es crucial para evitar engaños y asegurar la integridad de las comunicaciones.


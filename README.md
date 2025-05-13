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


#### Respuesta
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

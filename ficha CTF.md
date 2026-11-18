
# Sesión CTF 1 · Casos guiados

## Ficha 1 · Caso guiado 1: Hash MD5

> **Enunciado.** La contraseña es `LearnTheHashFunction`. Calcula su hash **MD5** y envíalo como
> `flag{md5}`.

**1. Tipo de reto:** [X] Hash ☐ Codificación ☐ Cifrado ☐ Combinado

*¿Por qué lo clasificáis así y no como codificación o cifrado?*

```
Por que estamos utilizando el metodo de MD5SUM y este genera un hash.
```

**2. Concepto.** *¿Qué es una función hash? ¿Qué significa que sea determinista, que tenga
longitud fija y que sea irreversible? ¿Para qué se usan los hashes en seguridad? ¿Qué longitud
tiene un hash MD5 y por qué ya no se considera seguro?*

```
Es un algoritmo matematico que transforma cualquier bloque de datos en una cadena de caracteres unica de longitud fija.
```

**3. Procedimiento.** Comando exacto y explicación de cada parte:

| Parte del comando | Qué hace |
|---|---|
|echo -n  | Muestra en pantalla , -n le indica que no añada un salto de línea|
| LearnTheHashFunction | Es el texto (la cadena de caracteres) al que se le va a calcular el hash |
| tubería o pipe | Redirige la salida estándar del comando anterior (echo) para que sea utilizada como la entrada del comando siguiente md5sum |
| md5sum |Es la herramienta que lee los datos que le llegan, calcula su huella digital utilizando el algoritmo criptográfico MD5 (que produce una cadena hexadecimal de 32 caracteres) y la muestra en pantalla. |

**4. Por qué lo hacemos así.** *¿Por qué `echo -n` y no `echo`? Haced estas pruebas y explicad
el resultado.*

| Prueba | Hash obtenido | ¿Coincide con el correcto? |
|---|---|---|
| `echo -n LearnTheHashFunction` |b2f2d6b27b264d83fe1abe0169b7613e |Si |
| `echo LearnTheHashFunction` (sin `-n`) |c6600000d8fbb39ad68728190fd3d7dd |No |
| `echo -n learnTheHashFunction` (minúscula) |81b55e8d26f83b740bbd34ebb417a597 |No |

```
Conclusión: El hash es sensible a pequeños cambios
```

**5. Uso de la IA**

| Pregunta que le hicimos | Qué aprendimos | ¿Se equivocó? ¿Cómo lo comprobamos? |
|---|---|---|
|Que era md5 , como actuaba en la consola, y como se genera el hash |Aprendimos como funciona el md5 |No se equivoco , probando en la maquina y comparando con diferentes fuentes|

**6. Flag:** `flag{b2f2d6b27b264d83fe1abe0169b7613e}`

**7. Verificación.** *¿Cuántos caracteres tiene un hash MD5? ¿Qué caracteres puede contener?*

```
Siempre tiene 32 caracteres, hexadecimales.
```

---

## Ficha 2 · Caso guiado 2: SHA-256 y después MD5

> **Enunciado.** La contraseña es `ThisIsAMoreSecureHashFunction`. Calcula su **SHA-256** y, sobre
> ese resultado, su **MD5**. Envía `flag{md5}`.

**1. Tipo de reto:** ☐ Hash ☐ Codificación ☐ Cifrado ☐ Combinado

```
Hash
```

**2. Concepto.** *¿Qué diferencia hay entre SHA-256 y MD5 en longitud de salida y en seguridad?
¿Qué significa encadenar funciones hash? ¿Importa el orden?*

```
En logintud de salida en md5 son 32 caracteres y en SHA-256 son 64 caracteres. Significa coger la salida de una funcion y volver hashearla con la otra función. Si importa el orden porque puede variar.
```

**3. Procedimiento.** Comando exacto y explicación de cada parte:

| Parte del comando | Qué hace |
|---|---|
|echo -n | Muestra en pantalla , -n le indica que no añada un salto de línea |
|ThisIsAMoreSecureHashFunction |Es el texto (la cadena de caracteres) al que se le va a calcular el hash |
|tubería o pipe |Redirige la salida estándar del comando anterior (echo) para que sea utilizada como la entrada del comando  |
| sha256sum|Es la herramienta que lee los datos que le llegan, calcula su huella digital utilizando el algoritmo criptográfico sha256 (que produce una cadena hexadecimal de 64 caracteres) y la muestra en pantalla. |
| awk `printf $1`| Divide el texto de salida en columnas separadas por los espacios y como hemos puesto el 1 , nos coge la primera columna que en este caso seria el hash|
| md5sum|Es la herramienta que lee los datos que le llegan, calcula su huella digital utilizando el algoritmo criptográfico MD5 (que produce una cadena hexadecimal de 32 caracteres) y la muestra en pantalla. |

**4. Por qué lo hacemos así.** *¿Qué muestra exactamente `sha256sum` además del hash? ¿Por qué hay
que quitarlo? ¿Para qué sirve `awk '{printf $1}'`?*

| Prueba | MD5 obtenido | ¿Coincide con el correcto? |
|---|---|---|
| Con `awk '{printf $1}'` |dd321a22229e0bbb5f8271e370b61eb0 |Si |
| Sin `awk` (`sha256sum \| md5sum`) |2451263d95f7cc49cdd7a5bb46436d39 |No |

```
Conclusión: Muestra el hash seguido de un espacio y un guion
```

**5. Uso de la IA**

| Pregunta que le hicimos | Qué aprendimos | ¿Se equivocó? ¿Cómo lo comprobamos? |
|---|---|---|
| Como funciona el sha256|Como se utiliza |No se equivoco , probando en la maquina y comparando con diferentes fuentes|

**6. Flag:** `flag{dd321a22229e0bbb5f8271e370b61eb0}`

**7. Verificación.** *¿Cuántos caracteres tiene el SHA-256 intermedio? ¿Y el MD5 final?*

```
El intermedio tiene 64 y el final tiene 32
```

---

## Ficha 3 · Caso guiado 3: romper un hash con un diccionario

> **Enunciado.** Encuentra el texto cuyo MD5 es `54f662a095fa3d5fbbdaac72d176701b`. Pásalo a
> mayúsculas y calcula su MD5. Envía `flag{md5}`.

**1. Tipo de reto:** ☐ Hash ☐ Codificación ☐ Cifrado ☐ Combinado

```
Hash
```

**2. Concepto.** *Si un hash es irreversible, ¿cómo es posible encontrar el texto original? ¿Qué
es un ataque de diccionario? ¿Qué hace CrackStation? ¿Qué es la «sal» (salt) y cómo evitaría
este ataque?*

```
A traves de ataque de diccionario si es debil. Un tipo de ataque que prueba combinaciones de caracteres.. CrackStation es una base de datos que almacena hashes y los compara con el que introduzcas. La salt es una cadena de caracteres aleatorios que se añade a una contraseña en texto plano antes de pasarla por la función hash. Se podria evitar haciendo una contraseña mas robusta y haciendo la tecnica de salting.
```

**3. Procedimiento.** Pasos y herramientas:

| Paso | Herramienta | Qué hace |
|---|---|---|
| 1 |aisecurityguard.io |Decodear hash|
| 2 |md5sum |Funcion hash |

**4. Por qué lo hacemos así.** *¿Por qué CrackStation encuentra esta contraseña? ¿La encontraría
si fuera `Xq7#mP2!vL9@`? ¿Qué os dice esto sobre cómo elegir contraseñas?*

```
La encuentra porque compara el hash precalculado que tiene en la base de datos con el hash que hemos introducido. Lo encontraria si lo tuviera en la base de datos. Cuanto mas hashemos las contraseñas mejor
```

**5. Uso de la IA**

| Pregunta que le hicimos | Qué aprendimos | ¿Se equivocó? ¿Cómo lo comprobamos? |
|---|---|---|
|Que es salt | Como funcionaba| No se equivocó|

**6. Flag:** `flag{f395885371dd0ad12136d8a733e05e22}`

**7. Verificación.** *¿Cómo podéis comprobar, sin fiaros de CrackStation, que el texto que os ha
dado es el correcto?*

```
Cogiendo ese texto y codificandolo nosotros mismos.
```

---

## Ficha 4 · Caso guiado 4: Base64

> **Enunciado.** Decodifica el contenido en Base64 del reto y obtendrás una contraseña. Envía
> `flag{md5}` con el MD5 de esa contraseña.

**1. Tipo de reto:** ☐ Hash [X] Codificación ☐ Cifrado ☐ Combinado

*¿Por qué Base64 no es un cifrado?*

```
Porque solo cambia la representaion de los datos
```

**2. Concepto.** *¿Qué es una codificación? ¿Qué caracteres usa Base64 y para qué sirve el `=` del
final? ¿Hace falta una clave para leerlo? ¿Dónde se usa Base64 en la vida real?*

```
La codificación es el proceso de transformar información de un formato o sistema a otro, siguiendo un conjunto de reglas o normas preestablecidas, para que pueda ser interpretada, transmitida o almacenada correctamente por diferentes sistemas. Letras mayúsculas: A a la Z (26 caracteres)
letras minúsculas: a a la z (26 caracteres) ,dígitos numéricos: 0 al 9 (10 caracteres). El = sirve como relleno. En los tokens de autentificacion como ejemplo.
```

**3. Procedimiento.** Comandos exactos y explicación de cada parte:

| Parte del comando | Qué hace |
|---|---|
| base64 -d fichero.txt| Muestra el texto |
| -d | Indica a la funcion que codifique el texto |
|fichero.txt  | archivo en base64|

**4. Por qué lo hacemos así.** *¿Cómo reconocisteis que era Base64? ¿Qué pasaría si una
aplicación guardara las contraseñas en Base64?*

```
Lo reconocimos gracias a los patrones que utiliza y por el igual del final. Pasaria que se podria descodificar.
```

**5. Uso de la IA**

| Pregunta que le hicimos | Qué aprendimos | ¿Se equivocó? ¿Cómo lo comprobamos? |
|---|---|---|
| Que representaba el = del final de la cadena| Lo que significaba | No|

**6. Flag:** `flag{cf9df460535b4ec175a464c6c120d3fe}`

**7. Verificación.** *¿Cómo comprobáis que habéis copiado la contraseña exacta, sin espacios ni
signos de más?*

```
Con la longitud de la cadena que da de resultado el base64
```

---

# Sesión CTF 2 · Retos autónomos

## Ficha 5 · Reto 5: otra huella distinta (SHA-1)

> **Enunciado.** La contraseña es `HashingIsOneWay2026`. Calcula su hash **SHA-1** y envía
> `flag{sha1}`.

**1. Tipo de reto:** [X] Hash ☐ Codificación ☐ Cifrado ☐ Combinado

```
...
```

**2. Concepto.** *¿En qué se diferencia SHA-1 de MD5 en longitud de salida? ¿Se considera seguro
SHA-1 hoy? ¿Por qué?*

```
La diferencia es que md5 produce un hash de 32 caracteres, mientras que sha-1 produce 4o caracteres. No se considera seguro ya que es vulnerable a ataques de colisión, es decir es facil encontrar dos entradas distintas que generen el mismo hash.
```

**3. Procedimiento.** Comando exacto y explicación de cada parte:

| Parte del comando | Qué hace |
|---|---|
|echo | imprime la salida |
| -n | quita el salto de linea al final|
|sha1sum | calcula el hash de la entrada con SHA-1 |

**4. Por qué lo hacemos así.** *¿Por qué el guion y los espacios de la salida no forman parte de
la flag? Calculad también el MD5 de la misma contraseña y comparad las dos salidas.*
```
Porque es metadato de la información de la terminal y no forma parte del Hash.
```

| Algoritmo | Hash obtenido | Nº de caracteres | Bits |
|---|---|---|---|
| MD5 | 1374cfc99a0de192a72ea8f627304d52| 32|128 |
| SHA-1 | 98666584dca50be8bfc35461f4eabe0e31b887ad| 40|160 |



**5. Uso de la IA**

| Pregunta que le hicimos | Qué aprendimos | ¿Se equivocó? ¿Cómo lo comprobamos? |
|---|---|---|
| que era un ataque de colisión |que era ese ataque y que era la vulnerabilidad de SHA-1 |no, lo buscamos en otras fuentes y en los apuntes |
| la diferencia de salida entre sha-1 y md5| cuantos bits usa cada uno para calcular y cuantos caracteres dan de salida|no, lo buscamos en otras fuentes y en los apuntes

**6. Flag:** `flag{98666584dca50be8bfc35461f4eabe0e31b887ad}`

**7. Verificación**

```
Hemos comprobado la longitud, calculado quitando las entradas.
```

---

## Ficha 6 · Reto 6: el mensaje en otro alfabeto (hexadecimal)

> **Enunciado.** Decodifica la cadena hexadecimal `4b6572636b686f666673` y envía
> `flag{palabra}`.

**1. Tipo de reto:** ☐ Hash [X] Codificación ☐ Cifrado ☐ Combinado


**2. Concepto.** *¿Qué es el hexadecimal? ¿Qué caracteres puede contener? ¿Cuántos caracteres
hexadecimales hacen falta para representar un byte? ¿Protege la información o solo la
representa?*

```
Es un sistema de numeración, los caracteres son del rango 0-9 y contiene las deltras en el rango a-f. Hacen falta 2 caracteres hexadecimales para un byte. Hexadecimal solamente la representa.
```

**3. Procedimiento.** Comando o herramienta y explicación:

| Parte del comando o paso | Qué hace |
|---|---|
|xxd | herramienta para crear volcados hexadecimales |
| -r | indica que realice la operacion inversa, osea de hexacedimal a texto |


**4. Por qué lo hacemos así.** *¿Cómo supisteis que estaba en hexadecimal? La cadena tiene 20
caracteres: ¿cuántas letras esperáis obtener y por qué? Relacionadlo con el caso guiado 4.*

```
Por que la cadena esta compuestas con numeros en rango 0-9 y a-f. Esperamos tener 10 letras ya que cada caracter ocupa 1 byte, y un hexadecimal 4 bits (la mitad)
```

**Para investigar.** La palabra que obtenéis es el apellido de una persona que enunció un
principio muy importante en criptografía. *¿Quién era y qué dice ese principio?*

```
Auguste Kerckhoffs (1835-1903) fue un lingüista y criptógrafo holandés que sentó las bases de la criptografía moderna en 1883 con su obra La cryptographie militaire.

Su famoso Principio de Kerckhoffs dice que:

Un sistema de cifrado debe ser seguro incluso si todo sobre él es público, excepto la clave.
```

**5. Uso de la IA**

| Pregunta que le hicimos | Qué aprendimos | ¿Se equivocó? ¿Cómo lo comprobamos? |
|---|---|---|
| Quien era Kerckhoffs | sus fundamentos | no |

**6. Flag:** `flag{Kerckhoffs}`

**7. Verificación**

```
Hemos verificado la longitud del resultado y volverlo a pasar a hexadecimal para ver si seguia dando lo mismo
```

---

## Ficha 7 · Reto 7: Base… ¿cuál? (Base32)

> **Enunciado.** Decodifica la cadena en **Base32** `MRSWMZLOONQV6ZLOL5YHE33GOVXGI2LEMFSA====` y
> envía `flag{texto}`.

**1. Tipo de reto:** ☐ Hash [x] Codificación ☐ Cifrado ☐ Combinado

```
Es codificación ya que usamos base32
```

**2. Concepto.** *¿En qué se diferencia Base32 de Base64? Comparad el alfabeto y el relleno con
`=`.*

| | Base64 | Base32 |
|---|---|---|
| Caracteres que usa | alfabeto completo | A-Z 2-7|
| Relleno | Si (completar multiplos de 4 caracteres)|Si (completar multiplos de 8 caracteres) |
| ¿Usa minúsculas? | si| no|

**3. Procedimiento.** Comando exacto y explicación de cada parte:

| Parte del comando | Qué hace |
|---|---|
| base32 -d|descodifica la cadena usando el algoridmo base32 |
| | |

**4. Por qué lo hacemos así.** *¿Qué os hizo sospechar que no era Base64? ¿Qué pasa si intentáis
decodificarla con `base64 -d`?*

```
Nos hizo sospechar debido a la asuencia total de minúsculas y simbolos especiales. El comando arroja un error de caracteres invalidos.
```

**Para investigar.** *El texto obtenido es un principio de seguridad. ¿Qué significa? Poned un
ejemplo aplicado a un instituto.*

```
Es una estrategia de ciberseguridad que consiste en desplegar múltiples capas de protección superpuestas de forma independiente, ejemplo:

En lugar de proteger los ordenadores con una única contraseña, se ponen varias barreras:

Firewall en la red.

Contraseña del usuario.

Doble factor de autenticación (2FA) para profesores.

Redes separadas para alumnos y administración.

Si un alumno rompe la primera barrera (la contraseña), las demás siguen protegiendo el sistema.

```

**5. Uso de la IA**

| Pregunta que le hicimos | Qué aprendimos | ¿Se equivocó? ¿Cómo lo comprobamos? |
|---|---|---|
| diferencias entre base32 y 64 | las diferencias que habian entre estas dos | no |

**6. Flag:** `flag{defensa_en_profundidad}`

**7. Verificación**

```
AL aplicar la cadena introducida la terminal lo devuelve de forma limpia y sin errores.
```

---

## Ficha 8 · Reto 8: el primer cifrado de la historia (ROT13)

> **Enunciado.** Descifra el texto `FrthevqnqCbeBfphevqnqAbShapvban`, cifrado con **ROT13**, y
> envía `flag{defensa_en_profundidad}`.

**1. Tipo de reto:** ☐ Hash ☐ Codificación [X] Cifrado ☐ Combinado

*¿Por qué esto sí es un cifrado y Base64 no lo era? ¿Cuál es la clave?*

```
Base64 es codificación (representación pública sin secreto); ROT13 es cifrado porque altera el mensaje mediante una regla paramétrica. Clave: Desplazamiento de 13 posiciones ($k = 13$).
```

**2. Concepto.** *¿Qué es el cifrado César? ¿Qué es ROT13? ¿Por qué aplicar ROT13 dos veces
devuelve el texto original? ¿Qué relación tiene el 13 con un alfabeto de 26 letras?*

```
Cifrado César: Sustitución de cada letra desplazándola un número fijo de posiciones.

ROT13: Un César con desplazamiento fijo de 13.

Aplicar dos veces: Acumula 26 posiciones ($13+13$), dando una vuelta completa al alfabeto y volviendo al original.

Relación con 26: 13 es exactamente la mitad de 26 ($26 / 2$), lo que hace que cifrar y descifrar sean la misma operación simétrica.
```

**3. Procedimiento.** Comando o herramienta y explicación:

| Parte del comando o paso | Qué hace |
|---|---|
| | |
| tr 'A-Za-z' 'N-ZA-Mn-za-m' |Rota cada letra 13 posiciones en el alfabeto. |

**4. Por qué lo hacemos así.** *¿Por qué el cifrado César es inseguro? ¿Cuántas claves posibles
tiene? ¿Cómo lo romperíais si no supierais que el desplazamiento es 13?*

```
Inseguridad: Espacio de claves muy pequeño y vulnerable al análisis de frecuencias.

Claves posibles: 25 claves útiles (en 26 letras).

Romperlo sin saber la clave: Mediante fuerza bruta probando las 25 combinaciones posibles.
```

**Para investigar.** *El texto descifrado está relacionado con el reto 6. ¿Qué relación tiene con
el principio de Kerckhoffs?*

```
El texto descifrado es defensa_en_profundidad. El Principio de Kerckhoffs establece que un sistema debe ser seguro aunque todo su diseño sea público, salvo la clave. La "seguridad por oscuridad" (confiar en ocultar el algoritmo) viola este principio.
```

**5. Uso de la IA**

| Pregunta que le hicimos | Qué aprendimos | ¿Se equivocó? ¿Cómo lo comprobamos? |
|---|---|---|
| Como descifrar una cadena con ROT13 en la terminal|A usar el comando tr para realizar la sustitución alfabética |no |

**6. Flag:** `flag{SeguridadPorOscuridadNoFunciona}`

**7. Verificación.** *Volved a aplicar ROT13 al resultado. ¿Qué obtenéis?*

```
Obtenemos de nuevo la cadena original cifrada.
```

---

## Ficha 9 · Reto 9: capa sobre capa (combinado) ⭐

> **Enunciado.** La cadena `NTA2NTZlNzQ2NTczNzQ2OTZlNjc0NTc0Njk2MzZm` está en **Base64**. Al
> decodificarla obtendrás **hexadecimal**; al decodificar este, una **palabra**. Calcula el
> **MD5** de esa palabra y envía `flag{md5}`.

**1. Tipo de reto:** ☐ Hash ☐ Codificación ☐ Cifrado [X] Combinado

*¿Qué técnicas de los retos anteriores se combinan?*

```
Se combinan dos técnicas de codificación/representación de datos: Base64 y hexadecimal, 
seguidas de una función de hash ( MD5).
```

**2. Concepto.** *¿Por qué se usan varias capas de codificación? ¿Hace eso la información más
segura?*

```
Se usan múltiples capas para empaquetar datos, transportarlos de forma segura a través de diferentes sistemas o protocolos, o simplemente como un método de ofuscación en retos de CTF. 

No, aplicar capas de codificación no hace la información más segura por sí misma, ya que la codificación  es reversible y no utiliza claves secretas (a diferencia del cifrado). Cualquiera puede revertir  las capas si conoce el orden y los formatos utilizados.
```

**3. Procedimiento.** Completad la tabla con cada capa:

| Capa | Lo que tenéis | Cómo reconocéis el formato | Comando | Resultado |
|---|---|---|---|---|
| 1 | `NTA2NTZl…` | Longitud múltiplo de 4, caracteres alfanuméricos con mayúsculas, minúsculas y = al final si aplica.| echo 'NTA2NTZlNzQ2NTczNzQ2OTZlNjc0NTc0Njk2MzZm' \| base64 -d| 50656e74657374696e67457469636f|
| 2 |50656e74657374696e67457469636f                                                                             |Cadena formada exclusivamente por caracteres hexadecimales (0-9, a-f) con una longitud par. | echo '50656e74657374696e67457469636f' \| xxd -r -p| PentestingEtico|
| 3 |PentestingEtico | Palabra en texto plano obtenida tras la decodificación hexadecimal.|echo -n 'PentestingEtico' \| md5sum | c784ae85751ffee70b053ec8ea30c92a|

**4. Por qué lo hacemos así.** *¿Cómo decidisteis el orden de las capas? ¿Qué pasaría si
intentarais decodificar el hexadecimal antes que el Base64?*

```
El orden se deduce analizando el formato externo inicial: la cadena original contenía letras mayúsculas y minúsculas típicas de Base64.

Si hubiésemos intentado decodificarla como hexadecimal primero, nos habría dado error o caracteres extraños porque los caracteres de Base64 incluyen letras como 'N', 'T', etc., que no son válidas en hexadecimal estricto. El flujo lógico exige deshacer primero la capa más externa (Base64) para revelar la capa interna (hexadecimal).
```

**5. Uso de la IA**

| Pregunta que le hicimos | Qué aprendimos | ¿Se equivocó? ¿Cómo lo comprobamos? |
|---|---|---|
|¿Cómo puedo decodificar una cadena en Base64 y luego su hexadecimal resultante desde la terminal de Linux? | aprendimos a concatenar comandos | no|

**6. Flag:** `flag{c784ae85751ffee70b053ec8ea30c92a}`

**7. Verificación**

```
Hemos hecho el proceso completamente a la inversa para ver si nos daba la misma cadena inicial
```

---

## Reflexión final del equipo (opcional, se valora en «Uso reflexivo de la IA»)

*En 5 a 10 líneas: ¿en qué momento la IA os ayudó a entender algo? ¿Os dio alguna respuesta
incorrecta que supisteis detectar? ¿Cómo lo hicisteis?*

```
La IA nos ayudó especialmente a recordar la sintaxis exacta de herramientas de línea de comandos como xxd para transformar hexadecimal a texto plano de forma rápida.
No cometió errores en los comandos básicos, pero aprendimos que siempre es crucial verificar manualmente el resultado de cada capa para asegurarnos de que la palabra obtenida ("pentestingetic") tiene sentido antes de calcular el hash final.
```

---

## Rúbrica con la que se corrige cada ficha

| Criterio | Peso | Nivel alto | Nivel medio | Nivel bajo |
|---|---|---|---|---|
| **Identificación del concepto** (apartados 1 y 2) | 25 % | Nombra el concepto con precisión y lo relaciona con otros retos | Identifica el concepto principal | No reconoce de qué trata el reto |
| **Justificación del procedimiento** (apartados 3 y 4) | 35 % | Explica el porqué de cada paso y de los detalles | Explica los pasos principales | Ejecuta comandos sin justificar |
| **Uso reflexivo de la IA** (apartado 5) | 20 % | Sus preguntas buscan entender y detecta errores de la IA | Usa la IA para apoyarse y entender | Copia respuestas de la IA sin entenderlas |
| **Resultado técnico** (apartados 6 y 7) | 10 % | Flag correcta y en el formato pedido | Flag correcta con un fallo menor de formato | Flag incorrecta |
| **Expresión y rigor** (toda la ficha) | 10 % | Vocabulario técnico correcto y claro | Se entiende, con imprecisiones menores | Vocabulario incorrecto o confuso |

Cada criterio se puntúa con la escala de 8 niveles: Excelente 10 · Sobresaliente 9 · Notable 7,5
· Bien 6 · Aprobado 5 · Insuficiente 2 · Muy insuficiente 0,5 · Sin respuesta 0.

# Fase 1 — Inventario de necesidades de comunicación

**Proyecto:** Pasarela Universal de Comunicación NWS — CASA VIVA  
**Estado:** PROPUESTA PARA REVISIÓN; no constituye arquitectura ni contrato aprobado.  
**Alcance:** exclusivamente Fase 1 de `PROMPT_INICIO_DESARROLLO.md`.  
**Fecha de elaboración:** 2026-10-13.

## 1. Pregunta y método

**Pregunta:** ¿qué operaciones y tipos de datos necesita Casa Viva para comunicarse con funciones distintas, sin presuponer cómo están conectadas?

Método: **necesidad → hipótesis → contraejemplo → prueba conceptual → corrección → criterio de aceptación**. Este documento no elige protocolos, equipos, interfaces de red, paquetes, drivers ni número de bloques de software.

La unidad semántica de interacción es una **función accesible** de una unidad física o lógica. Un mismo aparato puede ofrecer muchas funciones y cada una puede presentar posibilidades de lectura, escritura, ejecución y observación diferentes. La identidad del aparato no se confunde con la función o canal.

**Invariantes candidatos, independientes del transporte:**

1. No inventar funciones, lecturas, confirmaciones, precisión ni retornos ausentes.
2. No deducir efecto físico de aceptación, transmisión ni acuse del interlocutor.
3. No convertir ausencia/caducidad/desconocimiento en `false`, cero o éxito.
4. Distinguir acciones cuyo reintento podría duplicar un efecto de acciones cuyo objetivo es fijar un estado.
5. Conservar la identidad del dispositivo con independencia de su ruta de acceso.
6. Admitir valores, eventos y resultados posteriores con procedencia, temporalidad y significado explícitos.
7. La representación de una función exclusiva de un fabricante no presupone soporte técnico real de acceso.
8. No sustituir autoridades de decisión/ejecución ni el gemelo digital de Casa Viva.

## 2. Necesidades transversales (no son módulos)

| ID | Necesidad | Límite que debemos expresar |
|---|---|---|
| N01 | Dirigir una interacción a una **función** concreta de una unidad o canal identificable | El dispositivo puede tener múltiples funciones y canales; identidad física distinta de ubicación o ruta |
| N02 | Solicitar información cuando exista acceso de lectura | Puede no existir lectura, estar deshabilitada, quedar obsoleta o no responder |
| N03 | Solicitar un valor/estado objetivo | La petición no certifica el estado alcanzado; restricciones de rango, enumeración y unidad |
| N04 | Ejecutar procedimientos, impulsos o acciones con argumentos | Pueden ser irreversibles, relativos, no idempotentes, demorados o carecer de respuesta |
| N05 | Recibir observaciones espontáneas, periódicas, suscritas o continuas | No todo sensor admite lectura ni toda función admite suscripción |
| N06 | Identificar el significado de un dato | Booleanos, números, texto, enumeraciones, estructuras, eventos, datos binarios o flujos; unidades y escalas |
| N07 | Diferenciar cantidad instantánea, estado, consigna, acumulado y progreso | No son intercambiables aunque ambos sean números |
| N08 | Expresar capacidad ausente, inaccesible o desconocida | No declarar compatibles funciones sin acceso demostrado |
| N09 | Identificar **qué sabe** el sistema y **de dónde proviene** | Lo solicitado, admitido, transmitido, acusado, reportado y medido no son equivalentes |
| N10 | Expresar el carácter temporal de la información | Fecha de observación si se conoce, frescura, vigencia, retraso, secuencia parcial e incertidumbre |
| N11 | Representar resultados tardíos o cambios sin petición correspondiente | Una acción larga puede entregar progreso y resultado mucho después de aceptarse |
| N12 | Representar ausencia, pérdida o ambigüedad de respuesta | La petición puede haberse ejecutado aun cuando se pierda el retorno |
| N13 | Conocer si un reintento es semánticamente admisible | `SET ON` no equivale a `INVOKE toggle`; tampoco cualquier `SET` es automáticamente seguro sin conocer su semántica |
| N14 | Conservar datos específicos de una función sin reinterpretarlos falsamente | Argumentos, resultados y eventos propietarios con significado documentado |
| N15 | Expresar distintas fuentes de observación para la misma realidad | Estado declarado, lectura interna y medición independiente no confirman lo mismo |
| N16 | Expresar acceso restringido o temporalmente indisponible | Una capacidad existente no significa que esté disponible en toda condición |
| N17 | Tratar información simultánea, multicanal o agrupada | Un aparato puede exponer cientos de puntos; no es necesario multiplicar su identidad física |
| N18 | Gestionar coherentemente inicio/fin de una observación cuando el acceso lo permita | Una recepción pasiva no es lo mismo que una sesión de observación solicitada |
| N19 | Distinguir errores comprobables de resultados desconocidos | Error de formato, rechazo, indisponibilidad y respuesta extraviada tienen implicaciones diferentes |
| N20 | Conservar la naturaleza de flujos y objetos grandes | Una imagen o flujo de vídeo no se reduce a un número ni a un estado estático |

## 3. Inventario contrastado por familias

La tabla clasifica **necesidades**, no garantiza funcionalidades en ninguna marca ni aparato.

| Familia de función | Operaciones posibles, si están accesibles | Datos que deben preservarse | Contraejemplo decisivo |
|---|---|---|---|
| Relés, enchufes, conmutadores | Fijar ON/OFF; leer estado; observar cambios | Estado reportado, potencia instantánea, energía acumulada, disponibilidad de medición | Relé con ON/OFF, pero sin potencia: nunca inventarla ni tomar ON como consumo real |
| Iluminación y regulación | Fijar encendido, brillo/color, activar escena; leer u observar | Brillo porcentual o específico, temperatura de color, escena, transición | Una escena puede ser una acción, no un valor estable; cambio manual espontáneo |
| Persianas, accesos y motores | Abrir, cerrar, parar, mover a consigna, consultar posición si existe | Estado declarado, posición real/estimada, fin de carrera, movimiento | Motor sin sensor de posición: no fabricar un porcentaje exacto |
| Sensores y alarmas | Leer cuando sea posible; recibir muestras/eventos | Medidas, umbrales, detecciones, alarma, vigencia | Sensor que solo emite espontáneamente: `READ` no está disponible |
| Ventilación y climatización | Fijar modo/velocidad/consigna, activar funciones, leer/observar | Consigna frente a temperatura medida, modo, errores, velocidad | Consigna 21 °C no significa estancia a 21 °C |
| Energía, contadores y riego | Leer acumulados y tasas; fijar válvulas/consignas; iniciar procesos | Corriente, tensión, potencia, energía, caudal, volumen, estado válvula | Contador acumulativo no debe confundirse con flujo instantáneo |
| Multimedia y mandos unidireccionales | Ejecutar pulsaciones; fijar fuente/volumen si existe modo absoluto | Estado y volumen solo si son observables; confirmación del emisor | Mandar `power` no confirma encendido, y `toggle` no es `SET ON` |
| Robots y electrodomésticos | Iniciar, cancelar/parar, establecer programa; leer/observar | Progreso, estado de tarea, códigos de fallo, batería, finalización | Un `start` aceptado no implica una tarea terminada |
| Cámaras y audio | Obtener imagen, observar flujo/eventos, orientar si está permitido | Imagen binaria, secuencia audiovisual, eventos, orientación | Un flujo no se representa fielmente como lectura escalar repetida |
| PLC, controles y equipos multicanal | Leer/escribir puntos; ejecutar procedimientos; observar eventos | Canales, tipos heterogéneos, valores, diagnósticos | Cientos de canales bajo una sola unidad, sin crear cientos de aparatos físicos |
| Funciones especializadas o propietarias | Leer, fijar o invocar según semántica real de la función | Argumentos y salidas tipados, datos exclusivos de fabricante | Operación atómica propietaria con varios argumentos y múltiples resultados |
| Elementos puramente lógicos | Consultar/fijar/activar/observar cuando tengan interfaz real | Estados abstractos, eventos, resultados, identidad lógica | Una función lógica no requiere forzarle una carcasa física |

### Tipos y cualidades de datos necesarios

- **Valor simple:** booleano, numérico, cadena, enumeración, fecha/hora o identificador.
- **Valor compuesto:** varios campos heterogéneos con relaciones propias; admite parámetros y resultados complejos.
- **Magnitud física:** dimensión, unidad, escala, resolución y, cuando exista, incertidumbre.
- **Estado y consigna:** qué se reporta frente a qué se solicitó; no son sinónimos.
- **Medición:** instantánea, muestra periódica, máximo/mínimo, totalizador o derivada temporal; conservar el tipo temporal.
- **Evento:** ocurrencia con contenido propio, aun cuando no haya ninguna operación pendiente.
- **Proceso:** admisión, progreso, finalización o interrupción; estados provisionales que no equivalen a un resultado final.
- **Objeto binario o flujo:** imagen, audio, vídeo o datos opacos definidos por la función; no convertirlos por defecto en texto ni en una lectura escalar.
- **Ausencia de dato:** no disponible, no compatible, no recibido, caducado, inválido, desconocido; los motivos no son intercambiables.
- **Procedencia y calidad:** fuente, momento conocido, condición de observación y relación con una petición cuando sea demostrable. No presuponer marcas de tiempo ni causalidad exacta cuando el origen no las aporta.

Estas son categorías **semánticas**, no una propuesta de formato de paquete ni de almacenamiento.

## 4. Las cuatro primitivas: hipótesis sometida a refutación

| Primitiva candidata | Sentido que preserva | Dónde fallaría una interpretación ingenua | Corrección mínima propuesta |
|---|---|---|---|
| `READ` | Pedir una observación puntual de una función legible | Sensor solo emisor; petición sin respuesta; valor caducado | La lectura es **opcional** por función; el resultado puede quedar desconocido, no compatible o sin respuesta |
| `SET` | Solicitar fijar un estado o valor objetivo expresado en términos absolutos y válidos para esa función | Usar `SET` para `toggle`, pulsos o `incrementar 1` | Reservarlo para objetivos; operaciones relativas o con efecto procedimental pertenecen a `INVOKE` salvo semántica documentada equivalente |
| `INVOKE` | Solicitar un acto/procedimiento, posiblemente con argumentos y respuesta propia | Confundir aceptación con ejecución completa; repetir un impulso; forzar resultado inmediato | Admitir acción sin retorno y resultado/progreso diferido; tratar idempotencia según semántica demostrada |
| `OBSERVE` | Hacer posible la recepción de emisiones, cambios, eventos o flujos | Un evento espontáneo no es una orden; algunas fuentes no admiten solicitud de suscripción | Tratarlo provisionalmente como **modo de intercambio de información entrante**, no siempre como un comando enviado al dispositivo |

**Conclusión sobre el cuarteto:** las cuatro etiquetas cubren como **hipótesis útil de interacción** los casos examinados solo si se diferencia: (a) las tres intenciones activas `READ/SET/INVOKE`, (b) la observación entrante `OBSERVE`, que puede ser espontánea o solicitada, y (c) las respuestas, estados, eventos y confirmaciones, que **no son verbos adicionales**. No queda validado todavía como contrato técnico definitivo.

La existencia de una función no implica compatibilidad con las cuatro primitivas. Tampoco toda observación necesita una petición previa.

### Fronteras que no justifican, por ahora, un quinto verbo

- **Suscribirse o dejar de escuchar:** pueden ser gestiones de observación, si la función y la comunicación lo permiten. No imponerlas a un sensor de emisión espontánea.
- **Cancelar o detener:** cuando el dispositivo ofrece esas acciones, pueden modelarse como `INVOKE` de una función concreta; no inventar cancelación universal de procesos.
- **Pedir información sobre capacidades:** se requiere *conocer* cuáles existen, pero no se deduce que los aparatos admitan una acción universal de autodescripción. El descubrimiento automático está excluido.
- **Cambiar en incrementos o pulsar:** `INVOKE` expresa una acción relativa/no idempotente; no transformar en `SET` de un objetivo desconocido.
- **Lotes o transacciones atómicas:** si el aparato expone una operación indivisible con múltiples argumentos, se expresa como una función `INVOKE` sin fingir que varios `SET` independientes sean atómicos.
- **Errores y acuses:** son información sobre el intercambio, no operaciones domésticas nuevas.

## 5. Pruebas conceptuales de refutación

Estos resultados son **análisis semántico, no ensayos sobre hardware**.

| Prueba | Hipótesis que intenta romper | Resultado conceptual y corrección necesaria | Criterio de aceptación |
|---|---|---|---|
| T01 — Sensor de emisiones espontáneas | Toda función puede leerse | Falso; aceptar `OBSERVE` sin `READ` | Puede representar valores entrantes sin lectura ni solicitud previa |
| T02 — Mando de potencia sin retorno | Orden enviada = aparato encendido | Falso; `INVOKE` sin estado observado | Resultado físico permanece desconocido |
| T03 — Persiana sin posición | Movimiento ejecutado = posición exacta conocida | Falso; estado/posición pueden ser desconocidos | Nunca fabricar porcentaje |
| T04 — Tarea de media hora | Respuesta inmediata = final de tarea | Falso; progreso y finalización posteriores | Separar aceptación y fin real |
| T05 — Vídeo continuo | Todo dato cabe en `READ` puntual | Falso; observación con flujo | Preservar continuidad/estructura del dato |
| T06 — Muchos canales | Canal = aparato físico distinto | Falso | Una identidad física admite múltiples funciones/canales |
| T07 — Función de fabricante singular | Solo valen capacidades estándar | Falso | `INVOKE`, `SET` o `READ` pueden tener semántica específica tipada |
| T08 — Pulsación `toggle` | Toda orden es repetible | Falso; efecto duplicable | Sin repetición ciega ante respuesta perdida |
| T09 — Escritura de consigna | `SET` confirma efecto | Falso | Solicitud y estado/medición quedan diferenciados |
| T10 — Estado caducado | Último valor = valor actual | Falso | Conservar antigüedad y desconocimiento actual |
| T11 — Fallo tras transmisión | Sin respuesta = no ejecutado | Falso | Resultado de ejecución desconocido y reintento condicionado |
| T12 — Cambio manual no solicitado | Todo evento corresponde a una orden | Falso | Permitir eventos independientes |
| T13 — Datos en escala imprevista | Número = magnitud conocida | Falso | Rechazar o señalar ambigüedad; jamás reinterpretar sin base |
| T14 — Aparato accesible por varias vías | Ruta = identidad | Falso | Mantener identidad funcional y física sin duplicación |
| T15 — Acción transaccional propietaria | Una acción = un único argumento escalar | Falso | Permitir argumentos y resultados compuestos, sin inventar atomicidad propia |
| T16 — Fuente interna vs medición independiente | Ambas prueban el mismo hecho | Falso | Conservar origen y grado distinto de evidencia |
| T17 — Capacidad de solo escritura | `SET` necesita lectura de verificación | Falso | Expresar operación sin retorno e incertidumbre posterior |
| T18 — Una función desaparece temporalmente | Existir = estar disponible ahora | Falso | Separar soporte conocido, acceso actual y resultado de cada petición |

## 6. Qué puede saberse realmente de una operación

Una interacción puede proporcionar evidencias diferentes. **No constituyen una escalera automática de éxito:**

1. **Petición formulada/admitida:** Casa Viva o la pasarela ha entendido/aceptado la solicitud.
2. **Mensaje transmitido:** se tiene evidencia de que salió hacia un interlocutor.
3. **Acuse del interlocutor:** algún extremo respondió; quizá sea un intermediario.
4. **Ejecución declarada:** el interlocutor afirma que realizó la acción.
5. **Estado reportado:** existe lectura o evento sobre el estado de la función.
6. **Lectura interna:** instrumento o sensor del propio aparato informa una magnitud.
7. **Medición independiente:** una fuente independiente observa una consecuencia.
8. **Efecto físico confirmado:** solo cuando la evidencia concreta es suficiente para sostener la afirmación concreta.

No todas las rutas ofrecen todos esos niveles. Un acuse no equivale a medición independiente. Una lectura interna tampoco valida por sí sola cada efecto externo. Un resultado puede ser **indeterminado** aun cuando un mensaje haya sido transmitido y la acción efectivamente haya ocurrido.

No se diseña aquí un verificador físico nuevo: solo se impide que el intercambio pierda o fabrique distinciones de evidencia.

## 7. Condiciones mínimas de representación de una función

Para comprobar una función sin elegir todavía formatos, necesitamos poder expresar:

- **A qué corresponde:** unidad física/lógica, canal o endpoint y función precisa.
- **Qué permite:** lectura, fijación, invocación, observación, o cualquier subconjunto de ellas.
- **Qué significa:** nombre semántico, valores válidos, argumentos, unidades, escalas, significado de respuestas y eventos.
- **Qué tiempos maneja:** instantánea, impulsiva, eventual, de larga duración o continua; observación vigente, obsoleta o sin marca temporal fiable.
- **Qué garantías ofrece:** respuesta disponible o no, clases de evidencia conocidas, si un reintento puede causar otro efecto y bajo qué condiciones.
- **Qué no sabemos:** ausencia de acceso, lectura inexistente, idempotencia no documentada, respuesta ausente, ejecución indeterminada.
- **Qué conserva:** particularidades genuinas del fabricante sin convertirlas falsamente en capacidades genéricas.

Estas son preguntas que deben contestarse para cada caso real cuando corresponda; **no** son campos definitivos de un esquema, ni implican que un aparato publique todas esas propiedades.

## 8. Riesgos abiertos antes de aprobar la Fase 1

1. **Nombre/estatuto de `OBSERVE`.** Resulta útil semánticamente, pero debe decidirse si se conserva como cuarta primitiva o se presenta como **modalidad receptora** separada de las tres operaciones activas. No cambia la necesidad funcional; sí puede evitar la falsa idea de que todo evento comienza con una orden.
2. **Límites de `SET` frente a `INVOKE`.** La regla de estado objetivo versus acción es clara conceptualmente; al estudiar funciones reales habrá operaciones de interpretación ambigua que exigirán conservar su semántica específica en vez de fingir una equivalencia.
3. **Evidencia y resultado.** Debe decidirse el vocabulario operativo futuro sin anticipar códigos de error ni un contrato de paquetes. Es imprescindible preservar indeterminación e identificar el extremo que emite cada confirmación.
4. **Flujos y observaciones de duración indefinida.** El inventario reconoce su necesidad sin decidir todavía mecanismos de sesión, transporte o gestión.
5. **Disponibilidad de capacidades.** Que una función esté definida no implica que sea accesible en ese momento, por esa vía o con ese firmware; se necesitarán verificaciones empíricas más adelante.

Ninguno de estos puntos autoriza a diseñar hoy la Fase 2 o introducir componentes adicionales. Son los límites de la propuesta.

## 9. Conclusión y puerta de salida

**Resultado de la Fase 1: inventario elaborado y refutaciones conceptuales realizadas; fundamentos pendientes de evaluación del propietario.**

Las necesidades examinadas **no exigen todavía una quinta operación universal**. Provisionalmente, tres intenciones activas (`READ`, `SET`, `INVOKE`) más una modalidad de información entrante (`OBSERVE`) expresan los casos contrastados, **siempre** que:
- cada función declare solo las operaciones realmente accesibles;
- argumentos, resultados y eventos conserven tipos y significado, incluso propietario;
- se distingan ejecución, acuses, estados, mediciones y efectos físicos;
- se permita resultado desconocido, falta de respuesta y observación espontánea;
- se conserven identidad de aparato, canal y función sin confundirlos con una ruta;
- no se garanticen reintentos seguros ni lecturas inexistentes.

**Decisión solicitada al propietario:** revisar críticamente el inventario y la interpretación de `OBSERVE`; aceptar la Fase 1, o señalar un contraejemplo que obligue a corregirla. **No pasar a la Fase 2 antes de esa evaluación.**

**Estado de evidencia:** propuesta razonada; cero dispositivos probados, cero topologías aprobadas, cero integraciones verificadas, cero código desarrollado.

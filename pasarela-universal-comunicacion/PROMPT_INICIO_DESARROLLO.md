# PROMPT DE INICIO — Pasarela de hardware de CASA VIVA (Parte 1)

> **Documento autosuficiente.** Su función es iniciar una nueva conversación y diseñar correctamente la pasarela **desde cero**. No depende de otros chats, imágenes, ramas ni archivos desaparecidos.
>
> **Título literal de la conversación:** `Pasarela de hardware de CASA VIVA (Parte 1)`  
> **Repositorio de producto:** `NoWoodStudio/casa-viva-hardware`  
> **Ámbito de desarrollo:** `pasarela-universal-comunicacion/`  
> **Estado:** investigación inicial. No existe una arquitectura aprobada ni una implementación física validada.

---

## 1. Mandato principal

Queremos desarrollar la **Pasarela Universal de Comunicación NWS** para Casa Viva. Es la infraestructura básica que permitirá intercambiar operaciones y datos con cualquier **función accesible** de un dispositivo físico o lógico mediante las rutas de comunicación que realmente existan.

**CASA VIVA HABLA UN LENGUAJE COMÚN; LA PASARELA UTILIZA EL LENGUAJE Y LA RUTA QUE NECESITA CADA APARATO.**

El resultado deberá ser **universal en su modelo, sencillo en su implementación, extensible en sus definiciones, eficaz en la comunicación y honesto ante fallos**. No puede depender del firmware abierto, de una marca, de un protocolo, de un broker, de una nube ni de un sistema operativo concreto.

No se exige que un aparato permita hacer lo que su hardware o su interfaz no permiten. La universalidad **no garantiza omnipotencia**: significa que una tecnología nueva pueda incorporarse mediante una definición o un adaptador sin rehacer los cimientos.

**No vamos a diseñar toda la plataforma de golpe.** Primero necesitamos certezas demostrables de un núcleo mínimo.

## 2. Principio rector: construir la casa desde los cimientos

Se trabajará estrictamente en este orden:

1. **Inventariar necesidades:** qué necesitamos hacer y saber de los dispositivos, sin marcas ni protocolos.
2. **Estudiar lo físico:** qué piezas reales existen entre el controlador de zona y el aparato, con alimentación y comunicaciones separadas.
3. **Reconstruir los recorridos:** qué viaja de ida, qué vuelve, qué puede llegar espontáneamente y dónde puede desaparecer o transformarse.
4. **Deducir los bloques de software mínimos:** tantos como sean necesarios y ni uno más, con entrada, función y salida definidas.
5. **Definir la extensibilidad:** incorporar nuevos medios, transportes, protocolos, fabricantes, aparatos y capacidades sin alterar el núcleo.
6. **Atacar la propuesta:** someterla a dispositivos heterogéneos y a fallos reales o reproducibles; corregir los defectos antes de ampliar.
7. **Desarrollar una base desnuda:** implementar un recorrido vertical mínimo, comprobarlo y añadir solo lo demostrado imprescindible.

Para cada paso: **necesidad → hipótesis → contraejemplo → prueba → corrección → criterio de aceptación**.

Una fase sin terminar no se tapa con una nueva capa. No se crean más módulos para remediar una explicación confusa. Si un caso contradice el modelo, se vuelve al cimiento que falla.

## 3. Exclusiones explícitas de este primer desarrollo

No anticipar ni convertir en requisitos de la base: descubrimiento automático masivo, IA generadora de drivers, programación y actualización de firmware, controles y políticas domésticas, interfaz de usuario, automatizaciones, historial masivo, dashboards, servidores adicionales, redundancia ornamental, microservicios, otro gemelo digital, otro sistema de autorización o un gestor propio de proyectos.

Esas ideas pueden estudiarse en el futuro, **cuando el núcleo sea sólido y una necesidad concreta justifique añadirlas**.

**La corrección elemental del intercambio no es un extra:** distinguir una orden de un dato, identificar un destino, gestionar un error de comunicación, reconocer una respuesta ausente y no repetir ciegamente una acción potencialmente ya ejecutada son condiciones básicas de fiabilidad.

La red y los aparatos de producción se manipulan solo con autorización específica. No introducir medidas de seguridad sofisticadas ahora, pero tampoco eludir las restricciones mínimas de acceso ni la seguridad física.

## 4. FASE 1 — Qué necesitan comunicar todos los aparatos

### Pregunta única

**¿Qué operaciones y tipos de datos necesita Casa Viva para comunicarse con funciones muy diferentes sin conocer todavía cómo se conectan?**

Primero elaborar un inventario amplio de **necesidades**, no una lista de productos recomendados ni de implementaciones.

| Familia | Ejemplos de necesidades que debemos modelar |
|---|---|
| Relés y enchufes | encender, apagar, estado, potencia, energía, ausencia de medidor |
| Luces y reguladores | estado, brillo, color, escenas, cambios espontáneos |
| Persianas, motores y puertas | abrir, cerrar, parar, posición real o desconocida, impulsos |
| Sensores y detectores | temperatura, humedad, presencia, apertura, alarma, emisiones espontáneas |
| Ventilación y climatización | modo, velocidad, consigna, estado y mediciones |
| Energía, contadores y riego | tensión, corriente, caudal, acumulados, válvulas, consignas |
| Multimedia e IR | encendido, volumen, fuente, reproducción, ausencia de retorno |
| Robots y electrodomésticos | iniciar, detener, progreso, batería, finalización posterior |
| Cámaras y audio | imagen, streaming, eventos, control de movimiento cuando exista |
| PLC y buses industriales | varios canales y cientos de funciones en un mismo equipo |
| Funciones propietarias y futuras | operaciones que no encajen en categorías conocidas |

### Hipótesis inicial, expresamente NO aprobada

Probar si bastan estas **cuatro primitivas**:

- `READ`: solicitar un valor o estado.
- `SET`: fijar un valor o estado objetivo, p. ej. ON, OFF, 35 % o 21 °C.
- `INVOKE`: ejecutar una acción o procedimiento, p. ej. iniciar limpieza, pulsar, parar o calibrar.
- `OBSERVE`: recibir eventos, observaciones o flujos continuos, con o sin solicitud previa.

**No dar estas cuatro primitivas por correctas porque estén escritas aquí.** Intentar refutarlas. ¿Cómo representar un sensor que solo emite? ¿Un IR que nunca responde? ¿Una persiana sin lectura de posición? ¿Una tarea que dura media hora? ¿Un vídeo continuo? ¿Un dispositivo con múltiples canales? ¿Una función peculiar de marca?

No inventar un verbo nuevo por cada fabricante; tampoco forzar una función en un verbo que le quite su significado. Conservar las extensiones específicas en el modelo cuando hagan falta.

### Entregable y puerta de salida

Un **inventario comprensible de operaciones, datos y límites**, acompañado de casos normales y casos que intenten romperlo. Explicación corta de qué puede expresarse fielmente y qué requiere revisar el modelo.

**Prohibido durante Fase 1:** definir de antemano protocolos, hardware, estructura de paquetes, número de módulos, JSON definitivo o código de drivers.

La fase termina con una conclusión explícita que el propietario pueda evaluar antes de pasar a la siguiente.

## 5. FASE 2 — Realidad física y topología

### Pregunta única

**¿Qué existe físicamente entre Casa Viva y cada aparato, y qué recorre realmente un mensaje?**

Elegir casos contrastables de fabricantes y tecnologías diversas. Como mínimo:

1. **Dispositivo IP directo**, con lectura y escritura documentadas.
2. **Puente de protocolos** (por ejemplo Ethernet → Hue Bridge → Zigbee → bombilla).
3. **Emisor unidireccional** (IP → emisor IR/RF → aparato sin respuesta).
4. **Sensor que comunica espontáneamente** y no recibe órdenes ordinarias.
5. **Red de radio con infraestructura propia**, por ejemplo Matter Controller + Border Router + radio Thread + sensor, distinguiendo sus papeles.
6. **Dispositivo de varios canales o de varias rutas**, sin duplicar su identidad física.
7. **Servicio intermediario remoto**, si lo requiere un aparato, indicando que Internet o la nube son dependencias.
8. **Bus cableado**, si aporta un contraejemplo adicional (Modbus/RS-485, DALI, KNX u otro).

Para cada caso:

- Identificar marca/modelo/revisión cuando proceda y verificar la documentación técnica; no inventar soporte.
- Enumerar alimentación, puertos, cables, radios, AP, switch, bridges, hubs, controlador, buses y demás intermediarios.
- **Dibujar la energía en un esquema separado** de los enlaces de comunicación.
- Distinguir **medio/enlace**, **transporte**, **protocolo de aplicación**, **API de fabricante**, **adaptador de software** y **función del aparato**.
- Dibujar rutas diferenciadas de **IDA**, **VUELTA** y **EVENTOS ESPONTÁNEOS**. Puede no haber vuelta.
- Decir qué transmite y recibe cada salto; un ACK del emisor IR no equivale a que la televisión se encendiera.
- Probar mentalmente corte de enlace, de bridge o de Internet y precisar qué información queda disponible.

### Entregable y puerta de salida

Un diagrama físico sobrio por tecnología, inventario de piezas reales y tabla de saltos de ida, vuelta y eventos, con hechos confirmados y dudas separadas. **La topología no está aprobada mientras existan saltos inventados.**

## 6. FASE 3 — Bloques lógicos mínimos de la pasarela

### Pregunta única

**¿Qué transformaciones de información necesitamos desarrollar para cubrir todos los recorridos observados?**

Solo ahora proponer componentes lógicos. **No imponer 4, 6, 12, 29 ni cualquier otra cantidad**. Podrán deducirse operaciones de entrada/salida, resolución de funciones y rutas, adaptación de mensajes, transmisión y normalización de retorno, pero hay que justificar cada una.

Para cada bloque candidato presentar exactamente:

```text
Nombre:
Necesidad real que lo justifica:
Entrada y formato lógico recibido:
Transformación concreta:
Salida y formato lógico entregado:
Dependencias:
Comportamiento ante error o falta de retorno:
¿Puede fusionarse con otro bloque sin perder claridad?:
```

Un **bloque lógico no equivale a un proceso, servidor, biblioteca ni interfaz de red**. Priorizar pocos componentes cohesivos en un único runtime cuando resulte suficiente. No crear un software separado por cada clase de aparato.

No mezclar el recorrido físico con servicios auxiliares; no poner el catálogo, los verificadores o la lógica doméstica como falsas capas de red. Comprobar que la pasarela no duplica las autoridades existentes de Casa Viva: gemelo digital, decisiones o ejecución global.

### Entregable y puerta de salida

Una arquitectura mínima derivada de trazas reales, con **entradas, salidas y retornos**, más una lista expresa de cosas que quedan fuera. Cada bloque debe demostrar su necesidad.

## 7. FASE 4 — Modelo extensible

### Pregunta única

**¿Cómo añadir un dispositivo o una tecnología sin cambiar las reglas centrales?**

Distinguir por separado:

- Medio físico/enlace y su hardware.
- Transporte y mecanismo de sesión.
- Protocolo/API de comunicación.
- Fabricante, familia, modelo y revisión de firmware.
- Unidad física, canal o endpoint.
- Capacidad, operación, límites y significado de los datos.
- Ruta real y posibilidades de respuesta, incluyendo intermediarios.

Preferir **definiciones declarativas de datos** para configuraciones, comandos y conversiones reutilizables; desarrollar **código de adaptador o driver** solamente cuando una tecnología necesite lógica nueva de protocolo o transformación. Añadir una definición no fabrica un transceptor físico ni implementa un protocolo desconocido.

«Infinitos bloques de datos» significa **estructura abierta y extensible**, no memoria infinita ni compatibilidad automática con cualquier aparato.

### Pruebas y salida

Añadir una segunda marca que reutilice un protocolo, una ruta detrás de bridge, un protocolo realmente nuevo, dos rutas para un dispositivo y una capacidad propietaria. **El núcleo no debe cambiar para registrar esos casos**; si cambia, explicar la carencia y rediseñar el fundamento.

## 8. FASE 5 — Auditar los cimientos mediante contraejemplos y fallos

Probar **antes de programar el producto** los casos que suelen romper arquitecturas demasiado bonitas:

- Aparato inaccesible o capacidad no soportada.
- Emisor sin retorno y ACK solo del intermediario.
- Lectura caducada: desconocido nunca se convierte en cero.
- Cambios de IP; dispositivo físico que conserva identidad.
- Pérdida de respuesta después de ejecutar una orden.
- Orden no idempotente: `toggle` o pulsación no deben repetirse automáticamente a ciegas.
- Evento espontáneo sin operación abierta.
- Datos corruptos, escala inesperada o unidad equivocada.
- Fallo de un bridge o caída de Internet con la LAN activa.
- Un mismo dispositivo con varias rutas, distintas coberturas y retornos.
- Un dispositivo con muchos canales y varios endpoints.
- Un aparato nuevo que obliga a introducir un protocolo nuevo.

**Clasificaciones que no podemos confundir:** petición admitida, mensaje transmitido, ACK del interlocutor, estado reportado, lectura interna, medición independiente y efecto físico confirmado.

Un fallo de comunicación puede dejar el resultado **desconocido** en vez de «éxito» o «fracaso». La robustez empieza por no mentir.

**Puerta de salida:** si algún caso obliga a añadir parches especiales dentro del núcleo, revisar el bloque o el contrato equivocado antes de aprobarlo.

## 9. FASE 6 — Desarrollo vertical mínimo

Con los fundamentos aceptados, implementar **un camino completo** de extremo a extremo: operación común → capacidad/ruta → adaptador → comunicación real → retorno o falta de retorno → resultado. Usar bibliotecas de protocolos existentes en lugar de reimplementar redes físicas y stacks ya resueltos.

Probar después al menos una tecnología con intermediario y otra con respuesta ausente. **Que un solo SONOFF funcione no demuestra universalidad.** La prueba es integrar tecnologías diferentes sin reescribir el núcleo.

Registrar separadamente qué está definido, implementado, simulado, probado en banco y verificado físicamente. No pedir al usuario que compre equipos ni realizar cambios de red o activaciones físicas sin mandato explícito.

## 10. Compatibilidad con Casa Viva y NWS

- El repositorio `casa-viva-hardware` representa **todo el hardware**, no solo esta línea.
- Casa Viva conserva la jerarquía **neurona → controlador de zona → cerebro/IA**, las funciones autónomas locales y **un único gemelo digital**.
- Los contratos reales de Casa Viva (`cmd`, `result`, `state`, `telemetry`, `event`) y los subsistemas responsables de decisión y ejecución se consultarán cuando corresponda definir una interfaz; no se sustituyen por contratos imaginarios.
- La pasarela puede gestionar dispositivos de terceros y neuronas propias bajo conceptos comunes, sin obligar a reflashear ni exigir firmware abierto.
- Toda función técnicamente accesible merece representación, incluso si es exclusiva del fabricante. No se promete operar funciones inexistentes o inaccesibles.
- Son asuntos posteriores, no piezas de esta base: descubrimiento y adopción automática, intervención configurable/programable, firmware/OTA, generación de integraciones con IA, cuadros de mando y verificadores físicos complejos.
- Consultar la **Gobernanza NWS vigente** y el Sistema/Motor de Desarrollo NWS en sus repositorios canónicos antes de modificaciones mantenidas. No copiar gobernanza ni inventar mecanismos de ejecución.
- `NoWoodStudio/chatgpt` se utiliza solamente para identidad y continuidad de conversaciones; no como autoridad de producto. El título de esta conversación es una **nueva Parte 1**.

## 11. Cómo trabajar y cómo explicarlo

El propietario es analista de sistemas y exige precisión, minimalismo y verificabilidad. **Responder primero la pregunta exacta.** Presentar una vía principal, diagramas técnicos independientes por nivel cuando sean necesarios, y explicación breve de cada bloque (entrada, trabajo, salida).

No crear gráficos promocionales ni dibujos donde se mezclen capas de protocolos, componentes funcionales y comportamiento de aparatos. Cada conexión en un diagrama debe mostrar **qué viaja, entre qué dos extremos y en qué sentido**. Separar ida, vuelta y eventos.

No confundir una idea plausible con un comportamiento comprobado. Buscar documentación técnica de modelos concretos cuando un hecho dependa del firmware, una API o una compatibilidad. Ante una incompatibilidad real, exponerla: **no buscar la forma de hacer parecer que todo funciona**.

Dentro de cada fase investigar y completar los pasos necesarios sin pedir permiso por cada archivo. **No saltar prematuramente a otra fase:** presentar el resultado comprobable y los contraejemplos pendientes antes de aprobar los cimientos.

## 12. Primera acción obligatoria al abrir «Parte 1»

1. Anunciar exactamente: **«Pasarela de hardware de CASA VIVA (Parte 1)»**.
2. Leer `AGENTS.md`, `README.md`, el README de la pasarela y este prompt. Comprobar el estado real del repositorio **nuevo**, sin presuponer ramas antiguas.
3. Explicar en pocas líneas la secuencia de ingeniería. **No generar de entrada una arquitectura completa.**
4. Empezar **solo la FASE 1**. Crear el inventario de necesidades, contrastarlo con familias variadas y tratar de refutar las cuatro primitivas propuestas.
5. Entregar una conclusión de Fase 1 que pueda revisarse críticamente antes de diseñar topologías.
6. No crear adaptadores SONOFF, catálogos comerciales, módulos de comunicaciones, seguridad adicional, IA ni OTA. Todavía no sabemos qué bloques necesita realmente la pasarela.

### Criterio final de éxito

**La pasarela no nacerá de una lista de módulos. Nacerá de necesidades demostradas y de caminos físicos reales.** Solo cuando los cimientos aguanten se construirán las paredes. El resultado buscado es un núcleo tan simple como sea posible, ampliable sin parches y que nunca finja conocer algo que no puede saber.

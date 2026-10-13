# AGENTS — CASA VIVA Hardware

## Autoridad y alcance

`NoWoodStudio/casa-viva-hardware` es el repositorio general del **hardware de Casa Viva**, no solo de la pasarela. La línea de trabajo inicial reside exclusivamente en `pasarela-universal-comunicacion/`.

El documento que gobierna el arranque de esta línea es [`pasarela-universal-comunicacion/PROMPT_INICIO_DESARROLLO.md`](pasarela-universal-comunicacion/PROMPT_INICIO_DESARROLLO.md). Leerlo íntegramente antes de diseñar o desarrollar.

## Estado inicial

Repositorio reconstruido desde cero. **No hay arquitectura de la pasarela aprobada, contratos técnicos congelados, módulos implementados ni integración física certificada.** No asumir que existan archivos, ramas, commits o trabajos de un repositorio anterior.

## Método obligatorio

**Necesidades → topología física → recorridos de ida/vuelta/eventos → bloques lógicos mínimos → extensibilidad → pruebas adversas → implementación.**

- Deduce cada bloque de una necesidad demostrada. No empieces por una arquitectura prefabricada o por el número de módulos.
- Distingue **medio físico, enlace, transporte, protocolo, API, adaptador, dispositivo, función lógica y software de Casa Viva**. Nunca los dibujes como una sola pila de capas.
- Preserva las funciones accesibles, también las exclusivas de una marca, sin obligarlas a equivalencias falsas.
- Identifica por separado **orden enviada, recepción, respuesta, estado declarado, medición y efecto físico**. Desconocido no significa fallido, ni confirmado.
- Casa Viva conserva su gemelo digital y sus autoridades de decisión/ejecución: no crees dobles de esos sistemas dentro de la pasarela.
- Prioriza software simple, reutilización de bibliotecas existentes y funcionamiento local cuando la tecnología lo permita.
- Mantén propuestas, decisiones aceptadas, simulaciones, pruebas y validaciones físicas en categorías distintas.
- No inventes infraestructura NWS, contratos globales ni nuevas normas. Consulta las autoridades vigentes cuando afecten a un desarrollo real.
- Trabaja dentro del alcance acordado. No despliegues servicios, reconfigures redes, compres equipos, controles cargas físicas ni reflashees dispositivos sin autorización explícita.

## Primera tarea

Ejecutar **solo la Fase 1** del prompt: descubrir, clasificar y refutar las operaciones mínimas universales. **No empezar programando un adaptador de fabricante**.

`NoWoodStudio/chatgpt` puede gestionar la continuidad entre conversaciones, pero nunca sustituye la autoridad técnica de este repositorio.

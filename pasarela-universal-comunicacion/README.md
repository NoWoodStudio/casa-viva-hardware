# Pasarela Universal de Comunicación NWS

**Estado: fundamentos por descubrir y validar.** Este directorio es deliberadamente mínimo.

## Objetivo

Diseñar una vía universal y extensible para intercambiar operaciones y datos entre Casa Viva y las funciones accesibles de dispositivos diferentes, directamente o mediante intermediarios reales. Sin prometer acceso a interfaces inexistentes y sin falsear retornos, estados ni mediciones.

## Método de construcción

1. **Necesidades:** qué operaciones y datos existen realmente.
2. **Topologías físicas:** cables, radios, buses, red, bridges, aparatos y dependencias.
3. **Recorridos:** orden de ida, respuesta de vuelta y eventos sin solicitud previa.
4. **Bloques lógicos mínimos:** deducidos a partir de los recorridos, no inventados de antemano.
5. **Extensiones:** definiciones reutilizables de medio, protocolo, fabricante, modelo, función y ruta.
6. **Pruebas:** casos adversos, desconexiones, falta de retorno e incorporación de nuevas tecnologías.
7. **Implementación:** solo después de validar la base.

## Instrucción de arranque

**[Leer `PROMPT_INICIO_DESARROLLO.md`](PROMPT_INICIO_DESARROLLO.md)** íntegramente. Su primera tarea es exclusivamente la Fase 1.

No hay carpetas de arquitectura, drivers, protocolos, seguridad, catálogos ni planes adicionales porque **todavía no se ha demostrado que deban existir de esa forma**.

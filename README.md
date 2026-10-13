# CASA VIVA — Hardware

Repositorio general de hardware de Casa Viva: dispositivos comerciales, neuronas propias, controladores de zona, interfaces físicas y futuros desarrollos.

## Línea inicial: Pasarela Universal de Comunicación NWS

La finalidad de la pasarela es permitir que Casa Viva se comunique con **funciones técnicamente accesibles de aparatos muy diferentes**, sin hacer depender su núcleo de un fabricante, protocolo o medio físico concreto.

**Estado:** inicio desde cero. No hay arquitectura aprobada ni integraciones desarrolladas o probadas en este repositorio.

**Método:** necesidades → realidad física → caminos de comunicación → bloques lógicos mínimos → extensiones → pruebas → código.

### Empezar aquí

- [Instrucciones del repositorio](AGENTS.md)
- [Introducción a la pasarela](pasarela-universal-comunicacion/README.md)
- **[Prompt completo para abrir «Pasarela de hardware de CASA VIVA (Parte 1)»](pasarela-universal-comunicacion/PROMPT_INICIO_DESARROLLO.md)**

El directorio de la pasarela no contiene aún drivers, catálogos, esquemas congelados ni módulos. **Se crearán únicamente cuando los fundamentos los justifiquen.**

## Principios conservados

Casa Viva mantiene su jerarquía neurona → controlador de zona → cerebro y un único gemelo digital. La pasarela no replica sus decisiones ni su lógica doméstica. Puede integrar dispositivos comerciales con firmware abierto o cerrado, siempre que exista una vía de acceso real. La comunicación distingue lo declarado por un aparato de lo comprobado en la realidad.

La licencia del repositorio está **pendiente de decisión del propietario**. No se presupone licencia abierta por ausencia de un archivo `LICENSE`.

# Contribuyendo

Este proyecto es un trabajo en progreso. Las contribuciones son bienvenidas.

## Traducción

Las traducciones se hacen a partir del [repo original](https://github.com/hemanth/functional-programming-jargon). Los aportes de traducción pueden ser de diferentes maneras:

 1. Traducir los términos pendientes o nuevos.
 2. Corregir traducciones existentes.

Sin embargo, al traducir queremos utilizar un español internacional.

 > 💁 **Importante** Las definiciones originales (en inglés) están incluidas en el documento en forma de comentario. A medida que traduzcas una definición al español, entonces la remueves.

## Flujo de Trabajo

 1. Busca una traducción pendiente en la columna TODO: [ver traducciones pendientes](https://github.com/rmariuzzo/jerga-programacion-funcional/projects/3).
 2. Asegúrate que nadie esté trabajando en él (te puedes dar cuenta viendo que no haya nadie asignado ni nadie ha comentado que está trabajando).
 3. Sigue las reglas descritas abajo y crea tu PR.

## Reglas

 * Ejecuta `npm test` para hacer lint a los ejemplos de código. Tus cambios tienen que pasar.
 * No tocar la **Tabla de Contenido** (esta es regenerada automáticamente desde las definiciones existentes).
 * Si agregas un nuevo término al glosario o cambias el orden entonces ejecuta `npm run toc` para así regenerar la tabla de contenido.

Dicho esto, nos gustaría mantener un mínimo de consistencia a través del documento.

## Guía de estilo

 1. Cada definición debe tener al menos un ejemplo de código en JavaScript (ES2015).
 2. Las definiciones deben ser escritas utilizando las palabras más sencillas.
 3. Piense en los programadores que no tienen experiencia en programación funcional.
 4. Apreciamos más lo entendible que lo preciso.
 5. No use mucha jerga en las definiciones aunque estén definidas en otras parte del documento.
 6. Si utilizas una jerga crea un enlace a su definición en el documento.
 7. Evita bloques masivos de textos 😱.

## Convenciones de Código

[![JavaScript Style Guide](https://cdn.rawgit.com/feross/standard/master/badge.svg)](https://github.com/feross/standard)

 * Sé consistente con los demás ejemplos.
 * Utilice “arrow functions”.
 * Paréntesis siempre alrededor de los argumentos de funciones.
 * Incluye el resultado de la ejecución en comentarios.
 * KISS (Mantenlo corto y simple; _Keep it short and simple_)

Esta guía también es un trabajo en progreso. Sírvase en enviar PR!

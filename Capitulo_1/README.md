# LABORATORIO 1

Exploración interactiva de SharePoint moderno y SPFx

Duración: 60 minutos

## 1. Objetivo

Al finalizar, el participante habrá construido una primera comprensión práctica de SPFx a partir de la observación directa de SharePoint y de ejercicios de clasificación, comparación y toma de decisiones. No se crea todavía un proyecto SPFx; el desarrollo comenzará en el Laboratorio 2.

- Reconocer una página moderna de SharePoint y distinguir sus Web Parts.
- Comparar el modelo clásico de personalización con SPFx.
- Identificar los problemas que justifican evitar Custom Scripts.
- Observar dónde se ejecuta una solución SPFx y cómo interviene la identidad del usuario.
- Relacionar Node.js, NPM, TypeScript, React, Webpack y Heft con sus funciones.
- Identificar la relación de SPFx con SharePoint, Teams, Viva Connections y Microsoft Graph.
- Distinguir Web Parts de extensiones SPFx.
- Construir una primera visión de SPFx, Graph y Copilot en un escenario AI-first.
## 2. Contexto

SPFx representa un cambio desde personalizaciones clásicas hacia componentes client-side basados en tecnologías web modernas e integrados con Microsoft 365. En este laboratorio el concepto se comprobará mediante actividades breves sobre un sitio real de SharePoint, en lugar de limitarse a leer la descripción de la arquitectura.

El participante no modificará el sitio ni instalará soluciones.

## 3. Antes de comenzar

- Windows 11.
- Cuenta asignada para el laboratorio.
- Acceso al tenant de Microsoft 365.
- Acceso a SharePoint Online.
- Acceso al sitio SPFx Lab Site.
- Acceso a Microsoft Teams.
- Microsoft Edge o navegador equivalente.
Sitio sugerido: https://<tenant>.sharepoint.com/sites/SPFx-LabSite (utiliza la URL proporcionada por el instructor si es diferente).

## ACTIVIDAD 1 — Reconocer una página moderna de SharePoint

Identificar físicamente los elementos que forman una página moderna y distinguir cuáles funcionan como Web Parts.

### Paso 1. Abrir el entorno

Accede a Microsoft 365 y abre SharePoint Online → SPFx Lab Site. Localiza la página principal.

### Paso 2. Hacer un inventario

- Localiza el encabezado.
- Localiza la navegación.
- Identifica al menos dos secciones.
- Identifica al menos tres elementos de contenido.
- Selecciona dos elementos que consideres Web Parts.
### Paso 3. Registrar la observación

Para cada uno de los dos elementos seleccionados, escribe su nombre o una descripción breve y explica qué contenido presenta.

**Comprobación**

La respuesta debe basarse en elementos que realmente aparecen en la página, no en ejemplos del material.

**Evidencia**

Dos Web Parts identificados y descritos brevemente.

## ACTIVIDAD 2 — Clasificar una página: estructura y Web Parts

Distinguir la estructura de la página del contenido que aportan sus componentes.

### Paso 1. Seleccionar elementos

Elige cinco elementos visibles de la página anterior.

### Paso 2. Clasificarlos

Copia y completa la tabla. Marca cada elemento como estructura de página o como Web Part.

**Evidencia**

Tabla de clasificación completada.

## ACTIVIDAD 3 — Comparar SharePoint clásico y SPFx

Construir una comparación concreta entre los mecanismos clásicos y el modelo moderno de extensibilidad.

### Paso 1. Revisar las definiciones

Consulta el material del módulo para Farm Solutions, Add-ins, Custom Scripts y SPFx.

### Paso 2. Completar la comparación

No copies las definiciones completas. Escribe una frase propia que describa cómo se incorpora o ejecuta la personalización en cada modelo.

**Evidencia**

Una tabla comparativa con cuatro modelos y una descripción propia de cada uno.

## ACTIVIDAD 4 — Decidir si una personalización debe centralizarse

Reconocer las consecuencias prácticas de distribuir código independiente en muchos sitios y justificar la necesidad de una solución versionable.

### Paso 1. Analizar el escenario

Supón que una organización tiene 100 sitios SharePoint y cada sitio contiene una copia de un mismo script corporativo.

### Paso 2. Identificar consecuencias

- Escribe dos problemas que aparecerían al cambiar el script.
- Indica qué problema aparecería si una copia no se actualiza.
- Indica qué dificultad tendría TI para saber qué versión está instalada en cada sitio.
### Paso 3. Tomar una decisión

Explica en dos o tres líneas por qué un mecanismo centralizado y distribuible resulta más adecuado para este escenario.

**Evidencia**

Tres consecuencias identificadas y una decisión justificada.

Sitio sugerido: https://<tenant>.sharepoint.com/sites/SPFx-LabSite. Para las herramientas del navegador utiliza F12 sobre esa misma página.

## ACTIVIDAD 5 — Localizar la ejecución client-side

Comprobar mediante las herramientas del navegador que una página moderna contiene recursos que se ejecutan en el cliente y relacionar esa observación con SPFx.

### Paso 1. Observar la página

Abre las herramientas de desarrollo con F12. No modifiques el contenido.

### Paso 2. Inspeccionar

- Localiza elementos HTML.
- Identifica referencias a CSS.
- Identifica recursos JavaScript.
- Localiza el DOM de la página.
### Paso 3. Relacionar

Escribe una frase que explique por qué esta observación es consistente con el modelo client-side de SPFx.

**Evidencia**

Una observación escrita que relacione navegador, DOM, JavaScript y SPFx.

## ACTIVIDAD 6 — Resolver un caso de permisos

Aplicar el concepto de contexto de seguridad a una solución SPFx antes de trabajar con SharePoint REST y Microsoft Graph.

### Paso 1. Leer el caso

Un usuario abre una página que contiene una solución SPFx. El usuario no tiene permiso para consultar determinada información.

### Paso 2. Tomar una decisión

Indica si la solución debería mostrar esa información ignorando los permisos del usuario.

### Paso 3. Justificar

Explica la decisión utilizando los conceptos de identidad, permisos y datos disponibles.

**Comprobación**

La explicación debe dejar claro que SPFx trabaja dentro del contexto de seguridad de Microsoft 365.

**Evidencia**

Decisión y justificación de tres conceptos: identidad, permisos y datos.

## ACTIVIDAD 7 — Construir el mapa tecnológico de SPFx

Asignar una función concreta a cada tecnología del entorno de desarrollo y entender cómo colaboran para construir una solución.

### Paso 1. Clasificar

Completa la tabla con una función concreta para Node.js, NPM, TypeScript, React, Webpack y Heft.

### Paso 2. Ordenar mentalmente

Escribe una frase que explique cómo estas herramientas pasan de código fuente a una aplicación que puede utilizarse en SharePoint.

**Evidencia**

Tabla de funciones y una explicación breve del flujo tecnológico.

## ACTIVIDAD 8 — Diseñar una solución para Microsoft 365

Relacionar SPFx con los hosts y servicios de Microsoft 365 a partir de un caso de uso, en lugar de memorizar una lista de integraciones.

### Paso 1. Elegir necesidades

Imagina una aplicación corporativa que necesita mostrar información del usuario, acceder a documentos y estar disponible para usuarios de SharePoint y Teams.

### Paso 2. Asignar componentes

- Indica dónde podría presentarse la aplicación.
- Indica qué servicio podría proporcionar información del usuario y documentos.
- Indica qué papel tendría SPFx.
### Paso 3. Añadir Viva Connections

Indica qué cambiaría en tu respuesta si la misma experiencia también tuviera que estar disponible dentro de Viva Connections.

**Evidencia**

Una asignación de responsabilidades entre SPFx, SharePoint, Teams, Viva Connections y Microsoft Graph.

## ACTIVIDAD 9 — Elegir la extensión SPFx adecuada

Distinguir Web Parts y extensiones a partir del tipo de experiencia que se quiere modificar.

### Paso 1. Resolver tres casos

- Caso A: agregar información en un área general de una página o aplicación.
- Caso B: cambiar la representación visual de una columna.
- Caso C: agregar comandos disponibles para el usuario.
### Paso 2. Asignar la extensión

Para cada caso, selecciona entre Web Part, Application Customizer, Field Customizer y Command Set.

### Paso 3. Justificar

Escribe una línea explicando por qué elegiste esa opción.

**Evidencia**

Tres casos clasificados y justificados.

## ACTIVIDAD 10 — Analizar un escenario AI-first

Construir una primera relación entre SPFx, Microsoft Graph y Copilot respetando el contexto de seguridad.

### Paso 1. Analizar el escenario

Una aplicación SPFx muestra información corporativa obtenida mediante Microsoft Graph y posteriormente ofrece una experiencia de consulta con Copilot.

### Paso 2. Asignar responsabilidades

- Escribe qué responsabilidad corresponde a SPFx.
- Escribe qué responsabilidad corresponde a Microsoft Graph.
- Escribe qué responsabilidad corresponde a Copilot.
### Paso 3. Resolver el caso de seguridad

El usuario no tiene permiso para determinada información. Explica qué debería ocurrir antes de que esa información pueda utilizarse en una experiencia de IA.

**Evidencia**

Tres responsabilidades asignadas y una decisión de seguridad justificada.

## 4. Actividad final

Construye en una sola hoja un esquema textual de la solución que acabas de analizar. Debe contener:

- Un Web Part que represente una experiencia dentro de SharePoint.
- React y TypeScript como tecnologías de desarrollo.
- SharePoint como host y fuente de datos.
- Microsoft Graph como vía de acceso a información de Microsoft 365.
- Una extensión SPFx distinta de un Web Part, indicando qué problema resolvería.
- Copilot como capa de interacción inteligente.
- Una nota de seguridad que indique que la información disponible depende de la identidad y permisos del usuario.
El esquema debe explicar las relaciones entre los componentes con frases breves

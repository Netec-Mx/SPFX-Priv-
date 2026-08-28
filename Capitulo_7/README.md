# Laboratorio 

Migración de soluciones SharePoint clásicas a SPFx moderno

Duración: 120 minutos

## 1. Objetivo

Al finalizar el laboratorio, el participante será capaz de:

identificar personalizaciones clásicas de SharePoint;

reconocer las limitaciones de Script Editor Web Parts;

plantear su transformación a SPFx Web Parts;

reemplazar personalizaciones de Master Pages / Page Layouts mediante Application Customizers;

identificar alternativas modernas a Event Receivers;

diferenciar cuándo utilizar SPFx Extensions, Azure Functions o Power Automate;

implementar una estrategia moderna de Branding y UI;

reconocer la estructura de un proyecto SPFx basado en Gulp;

migrar el proceso de build hacia Heft;

validar que la solución moderna pueda ser compilada y empaquetada;

documentar la migración bajo un enfoque de mantenimiento y gobernanza.

El resultado será una visión completa del proceso:

PROYECTO CLÁSICO

Script Editor

Master Page

Page Layout

Event Receiver

Gulp

MIGRACIÓN

PROYECTO MODERNO SPFx

Web Part Extensions Heft

Microsoft 365

Esta transición es el eje del módulo: identificar la responsabilidad de cada personalización clásica y trasladarla al mecanismo moderno que corresponde.

## 2. Contexto

La organización tiene un portal de SharePoint construido durante varios años.

En el portal existen diferentes personalizaciones:

un Script Editor que muestra información de empleados;

modificaciones realizadas mediante Master Pages;

lógica asociada a eventos de listas;

personalizaciones visuales mediante HTML/CSS;

proyectos SPFx antiguos construidos con Gulp.

La organización desea modernizar el portal sin reconstruirlo completamente.

El equipo de desarrollo debe analizar cada personalización y determinar cuál es su equivalente moderno.

## 3. Arquitectura objetivo

Durante el laboratorio se utilizará la siguiente referencia:

SHAREPOINT ONLINE

Web Parts Extensions Procesos

App Field Cmd Azure

Cust Cust Set Function

/ Power

Automate

Heft

.sppkg

App Catalog

La documentación del módulo presenta precisamente una arquitectura moderna basada en SPFx Extensions para la capa de presentación y Azure Functions/Power Automate para procesos serverless.

## 4. Preparación del ambiente

### 4.1 Software requerido

Utilizar el ambiente establecido para el curso:

Importante

Para este laboratorio no se recomienda instalar una versión antigua de Node.js para ejecutar un proyecto SPFx legado.

La finalidad es estudiar y ejecutar la migración hacia el ambiente moderno.

Por ello, el proyecto legado será utilizado principalmente como fuente de análisis, mientras que la solución destino utilizará el ambiente actual del curso.

## 5. Preparación del proyecto legado

Antes de modificar el proyecto, identifica qué parte del código corresponde al comportamiento funcional y qué parte pertenece al antiguo proceso de construcción. El proyecto legado sirve como referencia para decidir qué debe conservarse y qué debe reemplazarse.

Por ejemplo:

LegacyEmployeePortal/

```text
src/
```

```text
config/
```

```text
gulpfile.js
```

```text
package.json
```

```text
tsconfig.json
```

Abrir el proyecto en Visual Studio Code.

```text
cd LegacyEmployeePortal
```

```text
code .
```

Antes de migrar una solución debemos conocer qué estamos migrando.

El objetivo no es simplemente eliminar gulpfile.js.

Primero debemos identificar:

- qué funcionalidad existe
- dónde está implementada
- qué dependencias utiliza
- qué partes corresponden a SharePoint clásico
- qué partes deben transformarse a SPFx moderno
## Actividad 1 — Inventario de la solución clásica

Identificar qué personalizaciones existen en la solución clásica y decidir qué mecanismo moderno asumirá cada responsabilidad.

Crea en el proyecto un archivo:

```text
MIGRATION.md
```

Agrega esta tabla:

| Personalización | Tecnología actual | Destino moderno |

|---|---|---|

| Listado de empleados | Script Editor | SPFx Web Part |

| Cabecera corporativa | Master Page | Application Customizer |

| Evento de documento | Event Receiver | Azure Function / Power Automate |

| Columna visual | HTML/JS | Field Customizer |

| Acción contextual | JavaScript | Command Set |

| Build | Gulp | Heft |

Para cada fila, añade una breve justificación basada en la responsabilidad de la personalización.

## Actividad 2 — Migrar un Script Editor a SPFx Web Part

Recrear la interfaz del Script Editor como un Web Part SPFx sin copiar literalmente la implementación clásica.

Crea un proyecto SPFx con plantilla React llamado:

EmployeeDashboard

Dentro del Web Part crea:

EmployeeList.tsx

Utiliza inicialmente estos datos de prueba:

```text
const employees = [
```

```text
{ name: "Ana López", department: "Legal", role: "Abogada" },
```

```text
{ name: "Carlos Pérez", department: "TI", role: "Arquitecto" }
```

Muestra Nombre, Departamento y Cargo mediante React.

Ejecuta el Web Part y comprueba que la interfaz represente la información del Script Editor.

En esta actividad los datos son estáticos. La conexión con SharePoint o Graph se incorporará en una etapa posterior.

## Actividad 3 — Migrar Master Page a Application Customizer

Reemplazar el banner de la Master Page mediante una extensión soportada de SharePoint.

Crea un proyecto de tipo:

Application Customizer

La clase principal debe extender:

```text
BaseApplicationCustomizer<TProperties>
```

Importa la base desde:

```text
@microsoft/sp-application-base
```

Obtén el PlaceholderProvider mediante:

```text
this.context.placeholderProvider
```

Utiliza el Top placeholder para insertar:

Portal Corporativo

Mensaje de seguridad

Ejecuta la extensión en el sitio de laboratorio y comprueba que el banner aparezca en la parte superior de la página.

## Actividad 4 — Migrar Event Receivers

Determinar qué parte de la lógica de un Event Receiver pertenece a la interfaz y qué parte debe ejecutarse como proceso.

Usa el escenario de la biblioteca Contratos. Cada documento debe tener Cliente, Fecha y Responsable.

Clasifica estas necesidades:

Mostrar al usuario una acción para validar un documento → Command Set.

Presentar visualmente el estado de una columna → Field Customizer.

Validar automáticamente un documento cuando se agrega → Azure Function / Power Automate.

Indica en MIGRATION.md qué mecanismo utilizarías para cada caso y por qué.

## Actividad 5 — Diseñar el reemplazo del Event Receiver

Diseñar el reemplazo del Event Receiver para la biblioteca Contratos.

Crea en MIGRATION.md una sección llamada Reemplazo del Event Receiver.

Define esta arquitectura:

Documento agregado → Webhook de SharePoint → Azure Function → Validación de metadatos → Notificación en Teams

Define la responsabilidad del Command Set:

Validar contrato

El Command Set inicia la acción y muestra el resultado al usuario; la validación corporativa se ejecuta fuera del navegador.

Indica qué información recibe el proceso y qué condición debe provocar una notificación.

## Actividad 6 — Branding moderno

Definir una estrategia de Branding moderno para sustituir personalizaciones visuales clásicas.

Completa en MIGRATION.md esta matriz y añade una justificación:

| Necesidad | Extensión |

|---|---|

| Banner corporativo | Application Customizer |

| Logo global | Application Customizer |

| Estado visual de columna | Field Customizer |

| Botón contextual | Command Set |

Para cada necesidad, indica qué superficie de SharePoint se modifica y qué parte de la experiencia del usuario cambia.

## Actividad 7 — Crear el Field Customizer

Implementar una presentación visual para la columna Nivel de Confidencialidad sin modificar el valor almacenado.

En la biblioteca Contratos utiliza la columna:

Nivel de Confidencialidad

Valores de prueba:

Alta

Baja

Configura el Field Customizer para mostrar:

Alta 🔒

Baja 🟢

Ejecuta la extensión y comprueba que el valor almacenado siga siendo Alta o Baja mientras cambia únicamente su presentación.

## Actividad 8 — Migración de Gulp a Heft

Analizar el toolchain antiguo y preparar la solución para el modelo moderno.

Abre el proyecto legado y localiza:

```text
gulpfile.js
```

Revisa las tareas disponibles, por ejemplo:

build

bundle

serve

package

Registra en MIGRATION.md qué tarea de Gulp realizaba cada función.

Después crea una copia de trabajo destinada a la migración. No modifiques la única copia del proyecto legado.

En la copia de trabajo identifica las dependencias y configuración relacionadas con Gulp que deberán sustituirse por la configuración compatible con Heft.

La solución destino debe utilizar el ambiente SPFx 1.23.2 del curso.

## Actividad 9 — Preparar Heft

Comprobar que la solución moderna dispone de la configuración necesaria para Heft.

```text
Localiza:
```

```text
heft.json
```

Revisa las tareas configuradas en el archivo y confirma que la configuración pertenece al toolchain instalado para SPFx 1.23.2.

No copies versiones históricas de plugins ni valores del ejemplo de documentación.

Registra en MIGRATION.md qué tareas del antiguo gulpfile.js quedan cubiertas por la configuración moderna.

## Actividad 10 — Ejecutar el build moderno

Compilar la solución con el toolchain moderno.

Desde la raíz del proyecto ejecuta:

```text
heft build --production
```

Espera a que el proceso termine correctamente.

Si se produce un error, corrige la configuración o dependencia indicada y repite el build.

Registra en MIGRATION.md la ejecución exitosa del build.

## Actividad 11 — Empaquetar

Generar el paquete de despliegue de la solución migrada.

```text
Ejecuta:
```

```text
heft package-solution --production
```

Comprueba que exista un archivo .sppkg en:

```text
sharepoint/solution/
```

Registra el nombre del archivo generado en MIGRATION.md.

## Actividad 12 — Validación de la migración

Comprobar que la migración no introdujo cambios de código no relacionados.

```text
Ejecuta:
```

```text
git status
```

Después:

```text
git diff
```

Revisa que los cambios correspondan al trabajo de migración.

Registra el resultado:

```text
git add .
```

```text
git commit -m "chore: migrate build toolchain to Heft"
```

Verifica el commit con:

```text
git log --oneline -5
```

## Actividad 13 — Documentar la migración

Completar la documentación de la migración con las decisiones y comprobaciones realizadas.

En MIGRATION.md agrega:

## Migraciones realizadas

- Script Editor → SPFx Web Part

- Master Page → Application Customizer

- Presentación de columnas → Field Customizer

- Acción de usuario → Command Set

- Event Receiver → Azure Function / Power Automate

- Gulp → Heft

## Validación

- Build de producción: OK

- Package: OK

- Archivo .sppkg: generado

- Código versionado: OK

Añade una sección Pendientes con cualquier elemento que no haya podido implementarse o validarse en el entorno del laboratorio.

## Actividad 14 — Análisis de arquitectura

Revisar las decisiones de arquitectura tomadas durante la migración.

En MIGRATION.md completa estas relaciones:

Script Editor → SPFx Web Part: interfaz de usuario controlada por una solución SPFx.

Master Page / Page Layout → Application Customizer: personalización global soportada.

Columna HTML/JS → Field Customizer: presentación de una columna.

Acción JavaScript → Command Set: acción contextual iniciada por el usuario.

Event Receiver → Azure Function / Power Automate: procesamiento que puede ejecutarse fuera del navegador.

Gulp → Heft: toolchain moderna para build y package.

Para cada relación, indica qué evidencia del laboratorio demuestra que la migración funciona.

## 21. Flujo completo de migración

El participante debe poder representar finalmente el proceso:

PROYECTO LEGADO

Script Editor Master Page Event Receiver

SPFx Web Part Application Azure Function /

Customizer Power Automate

SPFx Extensions

Heft

.sppkg

App Catalog

SharePoint / Teams

## 22. Evidencias del laboratorio

El participante deberá entregar:

1. Inventario

```text
MIGRATION.md
```

con la matriz:

Clásico Moderno

2. Web Part

Código del:

EmployeeDashboard

3. Application Customizer

Código de la extensión y evidencia del:

Top placeholder

4. Field Customizer

Evidencia de:

Alta 🔒

Baja 🟢

5. Command Set

Evidencia del comando:

Validar contrato

6. Arquitectura Event Receiver

```text
Diagrama:
```

SharePoint

Webhook

Azure Function

Graph

Teams

7. Heft

Evidencia de:

```text
heft build --production
```

8. Package

```text
Archivo:
```

*.sppkg

9. Git

Commit de la migración.

10. Documento

```text
MIGRATION.md
```

## 23. Resultado integral del laboratorio

Al finalizar, habrás analizado personalizaciones clásicas, seleccionado su equivalente moderno, implementado ejemplos con SPFx Extensions y trasladado el proceso de build de Gulp a Heft. La migración quedará respaldada por MIGRATION.md, un build de producción, un paquete .sppkg y un commit que registra los cambios.

Al terminar el laboratorio, el participante habrá recorrido la transición:

En particular:

Script Editor SPFx Web Part

Master Page / Page Layout Application Customizer

Personalización de columnas Field Customizer

Acciones de usuario Command Set

Event Receiver SPFx Extension o Azure Function / Power Automate

Gulp Heft

Y la migración termina con una solución que puede seguir el ciclo:

| Componente | Ambiente |
|---|---|
| Sistema operativo | Windows 11 |
| Node.js | 22.x LTS |
| SPFx | 1.23.2 |
| React | 17.0.1 |
| TypeScript | Compatible con SPFx 1.23.2 |
| Git | Instalado |
| Visual Studio Code | Instalado |
| Yeoman | Instalado |
| Heft | Instalado |
| SharePoint Online | Tenant de laboratorio |

| Necesidad | Extensión |
|---|---|
| Banner corporativo | Application Customizer |
| Logo global | Application Customizer |
| Estado visual de columna | Field Customizer |
| Botón contextual | Command Set |

| Modelo antiguo | Modelo moderno |
|---|---|
| gulpfile.js | heft.json |
| Scripts personalizados | Configuración declarativa |
| Gulp | Heft |
| Build disperso | Build estandarizado |
| Dependencias acopladas | Plugins |
| Integración CI/CD compleja | Integración con RushStack/CI/CD |

| Actividad | Resultado |
|---|---|
| Inventario | Personalizaciones clásicas identificadas |
| Script Editor | Web Part SPFx definido |
| Master Page | Application Customizer |
| Event Receiver | Arquitectura serverless definida |
| Branding | Estrategia basada en Extensions |
| Field Customizer | Columna con presentación moderna |
| Command Set | Acción contextual definida |
| Gulp | Toolchain antigua identificada |
| Heft | Build moderno configurado |
| Build | Compilación de producción exitosa |
| Package | .sppkg generado |
| Git | Migración registrada |
| Documentación | Estrategia de migración trazable |

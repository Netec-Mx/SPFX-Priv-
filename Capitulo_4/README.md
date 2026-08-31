# Laboratorio 4

Extensiones avanzadas con SPFx

Duración: 90 minutos

## 1. Objetivo general

Construir una solución de personalización para un sitio de SharePoint Online denominado Portal de Proyectos, utilizando las principales extensiones avanzadas de SPFx estudiadas en el Módulo 4:

Application Customizer

Field Customizer

Command Set

Agrupación de comandos y paneles personalizados

Navigation Customizer

SharePoint REST

Microsoft Graph como integración opcional

Manifiestos y propiedades de extensiones

Despliegue mediante App Catalog

El resultado final será una experiencia similar a:

PORTAL DE PROYECTOS Accesos rápidos ▼

Aviso corporativo / banner global

Inicio | Proyectos | Documentos | Reportes

LISTA: PROYECTOS

Proyecto Responsable Estado

Portal SPFx Ana ✓ Completado

Migración M365 Carlos ◷ En progreso

Automatización Miguel ○ Pendiente

[ Aprobar ] [ Rechazar ] [ Ver detalles ]

Detalles del proyecto

Nombre: Portal SPFx

Responsable: Ana

Estado: Completado

## 2. Contexto del laboratorio

En los módulos anteriores se trabajó con Web Parts SPFx. Un Web Part normalmente ocupa una zona específica de una página.

En este módulo se cambia el enfoque.

Ahora necesitamos modificar la experiencia general de SharePoint, sin tener que modificar el código interno de SharePoint.

Por ejemplo:

mostrar un mensaje corporativo en todas las páginas;

modificar visualmente una columna;

agregar comandos al menú de una lista;

abrir un panel con información del elemento seleccionado;

agregar accesos rápidos a aplicaciones corporativas.

Esto es precisamente lo que el módulo define como extensiones avanzadas de SPFx.

SharePoint Online contiene tres superficies principales que serán personalizadas: páginas, listas y navegación. Application Customizer actuará sobre la experiencia global, Field Customizer sobre las celdas de una columna y Command Set sobre las acciones disponibles para elementos de una lista. Navigation Customizer modificará los accesos de navegación. Estas extensiones pueden integrar React, Fluent UI, SharePoint REST y Microsoft Graph cuando la funcionalidad lo requiera.

SHAREPOINT ONLINE

Página Lista Navegación

Application Field + Navigation

Customizer Command Set Customizer

SPFx Extensions

React Fluent UI APIs

SharePoint Graph

REST

## 3. Temas del Módulo 4 cubiertos

0. Preparar el ambiente.

1. Crear el Site Collection y la lista Proyectos.

2. Crear el Application Customizer para el banner global.

3. Incorporar Fluent UI al Application Customizer.

4. Crear el Field Customizer para la columna Status.

5. Asociar el Field Customizer a la columna Status.

6. Crear el Command Set para Aprobar, Rechazar y Ver detalles.

7. Trabajar con el elemento seleccionado.

8. Agrupar los comandos relacionados.

9. Crear el panel de detalles.

10. Conectar Ver detalles con el panel.

11. Crear el Navigation Customizer.

12. Incorporar Fluent UI al Navigation Customizer.

13. Integrar SharePoint REST y Microsoft Graph.

14. Compilar cada extensión.

15. Generar los paquetes .sppkg.

16. Publicar las soluciones en el App Catalog.

17. Asociar las extensiones al sitio.

18. Ejecutar la prueba integral.

4. Representación general de las actividades

## ACTIVIDAD 0

Preparar las herramientas que utilizarás para crear y probar las extensiones SPFx.

Abre PowerShell y verifica Node.js y npm:

```text
node --version
```

```text
npm --version
```

El laboratorio utiliza Node.js 22 LTS.

Instala las herramientas globales necesarias:

```text
npm install @rushstack/heft yo @microsoft/generator-sharepoint --global
```

Comprueba Yeoman y Heft:

```text
yo --version
```

```text
heft --version
```

Comprueba Visual Studio Code:

```text
code --version
```

Crea la carpeta de trabajo:

```text
mkdir C:\SPFx
```

```text
cd C:\SPFx
```

```text
mkdir M4-Extensiones
```

```text
cd M4-Extensiones
```

## ACTIVIDAD 1

Crear Site Collection + lista Proyectos

## ACTIVIDAD 2

Application Customizer

Banner global

## ACTIVIDAD 3

Field Customizer

Estado visual

## ACTIVIDAD 4

Command Set

Aprobar / Rechazar / Detalles

## ACTIVIDAD 5

Panel personalizado

## ACTIVIDAD 6

Navigation Customizer

## ACTIVIDAD 7

Integración y despliegue

RESULTADO FINAL

Portal de Proyectos personalizado

## 5. Arquitectura de la solución

Para que una persona sin experiencia pueda seguir el laboratorio, se trabajará con proyectos separados.

Esto evita que un error en una extensión bloquee todas las demás.

Tendremos:

M4-SPFx-Extensions/

ApplicationCustomizer/

FieldCustomizer/

CommandSet/

NavigationCustomizer/

Al finalizar tendremos cuatro soluciones SPFx que pueden desplegarse independientemente.

## ACTIVIDAD 0 — Preparar el ambiente

### Objetivo

Verificar que el equipo tiene las herramientas necesarias para desarrollar extensiones SPFx.

El contenido del módulo presupone conocimientos de desarrollo SPFx, TypeScript, React y Fluent UI. La documentación actual de Microsoft también establece como prerrequisitos experiencia con JavaScript/TypeScript/Node.js, Visual Studio Code y un tenant de Microsoft 365.

### 0.1. Software necesario

Se utilizará:

- Windows 10/11 o equivalente;
- Node.js LTS;
- npm;
- Visual Studio Code;
- SharePoint Online;
- Microsoft 365 tenant;
- permisos para utilizar un App Catalog;
- navegador moderno.
Para proyectos SPFx actuales, Microsoft documenta el toolchain basado en Heft desde SPFx 1.22; para esa línea se utiliza Node.js 22 LTS.

### 0.2. Verificar Node.js

Abrir PowerShell:

```text
node --version
```

Después:

```text
npm --version
```

SPFx se desarrolla sobre Node.js y utiliza npm para instalar las dependencias.

Si el comando:

node

no existe, primero debe instalarse Node.js.

### 0.3. Instalar las herramientas

Para el toolchain actual documentado por Microsoft:

```text
npm install @rushstack/heft yo @microsoft/generator-sharepoint --global
```

Verificar:

```text
yo --version
```

```text
heft --version
```

gulp --version

Nota: el Módulo 4 no define una versión concreta de Node.js ni un procedimiento de instalación. Por eso esta parte utiliza la preparación oficial actual de Microsoft y se separa del contenido conceptual del PDF. El documento del módulo se concentra en las extensiones y no en la instalación del toolchain.

### 0.4. Instalar Visual Studio Code

Instalar VS Code y comprobar:

```text
code --version
```

Microsoft utiliza Visual Studio Code en su documentación de preparación de SPFx.

### 0.5. Crear carpeta de trabajo

```text
mkdir C:\SPFx
```

```text
cd C:\SPFx
```

```text
mkdir M4-Extensiones
```

```text
cd M4-Extensiones
```

C:\SPFx\M4-Extensiones

## ACTIVIDAD 1 — Preparar SharePoint

### Objetivo

Crear el sitio y la lista que utilizarán todas las extensiones.

### 1.1. Crear Site Collection

Crear un sitio de SharePoint Online:

Nombre:

Portal de Proyectos

Por ejemplo:

/sites/PortalProyectos

### 1.2. Crear la lista

Dentro del sitio:

Nuevo Lista Lista en blanco

Nombre:

Proyectos

### 1.3. Crear columnas

Configurar:

Para Status:

- Pendiente
- En progreso
- Completado
- Rechazado
### 1.4. Crear datos de prueba

Agregar:

Porque las extensiones no solamente modifican páginas.

El Field Customizer y el Command Set trabajan directamente sobre el ListView de SharePoint.

## ACTIVIDAD 2 — Application Customizer

### Objetivo

Crear una extensión que inserte un mensaje global en SharePoint.

### 2.1. Crear proyecto

Desde PowerShell:

```text
cd C:\SPFx\M4-Extensiones
```

```text
mkdir ApplicationCustomizer
```

```text
cd ApplicationCustomizer
```

```text
yo @microsoft/sharepoint
```

Cuando aparezcan las preguntas del generador, selecciona una extensión y después el tipo Application Customizer.

Selecciona:

Application Customizer

Nombre:

```text
PortalBanner
```

### 2.2. Abrir proyecto

```text
code .
```

### 2.3. Identificar archivo principal

Localizar:

src

extensions

portalBanner

```text
PortalBannerApplicationCustomizer.ts
```

### 2.4. Entender la clase

La clase debe extender:

BaseApplicationCustomizer

La estructura conceptual es:

```text
export default class PortalBanner
```

extends BaseApplicationCustomizer<IProperties> {

```text
public onInit(): Promise<void> {
```

// lógica

```text
return Promise.resolve();
```

```text
}
```

```text
}
```

### 2.5. Crear las propiedades

```text
export interface IPortalBannerProperties {
```

```text
TopMessage: string;
```

```text
BottomMessage: string;
```

```text
}
```

Así el texto no queda completamente fijo dentro del código.

Tenemos:

Manifiesto

Propiedades

Application Customizer

Mensaje

El ejemplo del módulo utiliza precisamente propiedades TopMessage y BottomMessage.

### 2.6. Obtener el mensaje

Dentro de onInit():

```text
const topMessage: string =
```

```text
this.properties.TopMessage ||
```

```text
"Bienvenido al Portal de Proyectos";
```

```text
const bottomMessage: string =
```

```text
this.properties.BottomMessage ||
```

```text
"Información corporativa";
```

El operador || permite utilizar un valor predeterminado si no se proporcionó una propiedad.

### 2.7. Crear la barra superior

```text
const header = document.createElement("div");
```

header.innerText = topMessage;

header.style.padding = "10px";

header.style.background = "#0078d4";

header.style.color = "white";

header.style.textAlign = "center";

```text
document.body.insertBefore(
```

header,

```text
document.body.firstChild
```

);

En este paso se realiza lo siguiente:

SharePoint

Application Customizer

```text
document.body
```

Nuevo elemento HTML

### 2.8. Crear footer

```text
const footer = document.createElement("div");
```

footer.innerText = bottomMessage;

footer.style.padding = "8px";

footer.style.background = "#333";

footer.style.color = "white";

footer.style.textAlign = "center";

```text
document.body.appendChild(footer);
```

Esto sigue el patrón mostrado en el módulo para insertar un footer global.

## ACTIVIDAD 3 — Application Customizer con Fluent UI

### 3.1. Instalar Fluent UI si el proyecto no lo incluye

```text
npm install @fluentui/react
```

### 3.2. Importar

```text
import {
```

MessageBar,

MessageBarType

```text
} from '@fluentui/react';
```

```text
import * as React from 'react';
```

```text
import * as ReactDom from 'react-dom';
```

### 3.3. Crear contenedor

```text
const bar = document.createElement("div");
```

### 3.4. Renderizar MessageBar

```text
ReactDom.render(
```

```text
<MessageBar
```

messageBarType={MessageBarType.info}

>

Bienvenido al Portal de Proyectos

```text
</MessageBar>,
```

bar

);

### 3.5. Insertarlo

```text
document.body.insertBefore(
```

bar,

```text
document.body.firstChild
```

);

En las páginas de SharePoint aparecerá:

ℹ Bienvenido al Portal de Proyectos

Finalidad

Demostrar que una extensión SPFx puede combinar:

sin modificar el núcleo de SharePoint.

## ACTIVIDAD 4 — Field Customizer

### Objetivo

Modificar visualmente la columna Status de la lista Proyectos.

El Field Customizer no cambia el dato almacenado. Cambia cómo se presenta. El módulo destaca precisamente esta diferencia.

Antes:

- Completado
- En progreso
- Pendiente
Después:

✓ Completado

◷ En progreso

○ Pendiente

### 4.1. Crear proyecto

Desde:

```text
cd C:\SPFx\M4-Extensiones
```

```text
mkdir FieldCustomizer
```

```text
cd FieldCustomizer
```

```text
yo @microsoft/sharepoint
```

Seleccionar:

Extension

Field Customizer

Nombre:

```text
StatusFieldCustomizer
```

### 4.2. Localizar clase

Buscar:

src/extensions/statusFieldCustomizer/

La clase principal debe extender:

BaseFieldCustomizer

### 4.3. Implementar onRenderCell()

```text
public onRenderCell(event: any): void {
```

```text
const status: string = event.fieldValue;
```

```text
const element =
```

```text
React.createElement(
```

StatusBadge,

```text
{ status }
```

);

```text
ReactDom.render(
```

element,

```text
event.domElement
```

);

```text
}
```

En este caso, significa lo siguiente:

Por cada celda:

Celda SharePoint

onRenderCell()

```text
event.fieldValue
```

StatusBadge

### 4.4. Crear StatusBadge

Crear:

components/StatusBadge.tsx

Código:

```text
import * as React from 'react';
```

```text
import { Label } from '@fluentui/react';
```

```text
export default function StatusBadge(
```

```text
{ status }: { status: string }
```

) {

```text
const color =
```

status === 'Completado'

? 'green'

: status === 'En progreso'

? 'orange'

: 'gray';

```text
return (
```

```text
<Label
```

styles={{

root: {

color,

fontWeight: 'bold'

```text
}
```

```text
}}
```

>

```text
{status}
```

```text
</Label>
```

);

```text
}
```

### 4.5. Agregar iconos

Podemos enriquecerlo con:

```text
import {
```

Icon,

Label

```text
} from '@fluentui/react';
```

Y:

```text
<Icon
```

iconName={

status === 'Completado'

? 'CheckMark'

: 'Clock'

```text
}
```

/>

### 4.6. Implementar onDisposeCell()

```text
public onDisposeCell(event: any): void {
```

```text
ReactDom.unmountComponentAtNode(
```

```text
event.domElement
```

);

```text
super.onDisposeCell(event);
```

```text
}
```

Porque SharePoint puede eliminar o volver a crear las celdas.

Debemos limpiar el componente React cuando la celda deja de existir.

## ACTIVIDAD 5 — Asociar el Field Customizer a la columna

El Field Customizer necesita saber:

```text
"¿A qué columna debo aplicar?"
```

En nuestro caso:

Lista:

Proyectos

Columna:

Validación

Abrir:

Portal de Proyectos

Proyectos

Proyecto Owner Status

Portal SPFx Ana ✓ Completado

Migración M365 Carlos ◷ En progreso

Automatización Miguel ○ Pendiente

## ACTIVIDAD 6 — Command Set

### Objetivo

Agregar comandos personalizados a la lista:

- Aprobar
- Rechazar
- Ver detalles
Un Command Set permite agregar acciones personalizadas a los elementos de una lista o biblioteca. En este laboratorio se utilizará para aprobar, rechazar y consultar el proyecto seleccionado.

### 6.1. Crear proyecto

```text
cd C:\SPFx\M4-Extensiones
```

```text
mkdir CommandSet
```

```text
cd CommandSet
```

```text
yo @microsoft/sharepoint
```

Seleccionar:

Extension

ListView Command Set

Nombre:

ProjectCommandSet

### 6.2. Identificar clase

La clase debe extender:

BaseListViewCommandSet

### 6.3. Identificar comandos

Vamos a utilizar:

COMMAND_APPROVE

COMMAND_REJECT

COMMAND_DETAILS

Estos identificadores estarán definidos en el manifiesto.

### 6.4. Implementar onExecute()

```text
public onExecute(event: any): void {
```

switch (event.itemId) {

case 'COMMAND_APPROVE':

```text
Dialog.alert(
```

```text
'Elemento aprobado'
```

);

break;

case 'COMMAND_REJECT':

```text
Dialog.alert(
```

```text
'Elemento rechazado'
```

);

break;

case 'COMMAND_DETAILS':

```text
Dialog.alert(
```

```text
'Mostrando detalles del elemento'
```

);

break;

```text
}
```

```text
}
```

## ACTIVIDAD 7 — Trabajar con el elemento seleccionado

Ahora el comando mostrará información del proyecto seleccionado.

Queremos conocer el proyecto seleccionado.

Utilizaremos:

```text
event.selectedRows[0]
```

Por ejemplo:

```text
const selectedItem =
```

```text
event.selectedRows[0];
```

```text
const title =
```

```text
selectedItem.getValue('Title');
```

Después:

```text
Dialog.alert(
```

`Proyecto seleccionado: ${title}`

);

El Command Set trabaja sobre los elementos seleccionados.

```text
event.selectedRows[0]
```

y:

getValue('Title')

para obtener información del elemento.

## ACTIVIDAD 8 — Agrupar comandos

Configurar los comandos relacionados bajo un mismo grupo de acciones.

Localiza el manifiesto del Command Set y revisa la definición de los comandos.

Utiliza los identificadores:

COMMAND_APPROVE

COMMAND_REJECT

COMMAND_DETAILS

Configura el grupo con un nombre descriptivo, por ejemplo Acciones del proyecto.

Después de desplegar la extensión, selecciona un elemento de la lista Proyectos y comprueba que los tres comandos aparezcan dentro del mismo contexto.

## ACTIVIDAD 9 — Crear el panel de detalles

### Objetivo

Al seleccionar:

- Ver detalles
queremos abrir un panel lateral.

Detalles del proyecto X

Nombre: Portal SPFx

Propietario: Ana

Estado: Completado

### 9.1. Crear componente

Crear:

components/DetailsPanel.tsx

### 9.2. Código

```text
import * as React from 'react';
```

```text
import {
```

Panel,

Text

```text
} from '@fluentui/react';
```

```text
export default function DetailsPanel(
```

```text
{ item, onDismiss }: any
```

) {

```text
return (
```

```text
<Panel
```

isOpen={true}

onDismiss={onDismiss}

headerText="Detalles del proyecto"

>

```text
<Text>
```

Nombre:

```text
{item.getValue('Title')}
```

```text
</Text>
```

```text
<br />
```

```text
<Text>
```

Propietario:

```text
{item.getValue('Owner')}
```

```text
</Text>
```

```text
<br />
```

```text
<Text>
```

Estado:

```text
{item.getValue('Status')}
```

```text
</Text>
```

```text
</Panel>
```

);

```text
}
```

La estructura está basada en el ejemplo incluido en el módulo.

## ACTIVIDAD 10 — Conectar Ver detalles con el panel

La interacción sigue esta secuencia: el usuario selecciona un proyecto, elige Ver detalles, onExecute() identifica la acción, selectedRows[0] proporciona el elemento y DetailsPanel muestra sus datos mediante Fluent UI.

Usuario selecciona proyecto

- Ver detalles
onExecute()

selectedRows[0]

DetailsPanel

Fluent UI Panel

Validación

Seleccionar:

Portal SPFx

Después:

- Acciones del proyecto
- Ver detalles
Comprueba que aparezca:

Detalles del proyecto

Nombre: Portal SPFx

Propietario: Ana

Estado: Completado

## ACTIVIDAD 11 — Navigation Customizer

### Objetivo

Agregar accesos rápidos a la navegación.

### 11.1. Crear proyecto

```text
cd C:\SPFx\M4-Extensiones
```

```text
mkdir NavigationCustomizer
```

```text
cd NavigationCustomizer
```

```text
yo @microsoft/sharepoint
```

Seleccionar:

Extension

Application Customizer

Nombre:

PortalNavigation

Importante

### 11.2. Crear propiedades

```text
export interface INavigationCustomizerProperties {
```

```text
Links: string[];
```

```text
}
```

### 11.3. Leer navegación

El ejemplo del módulo utiliza:

```text
const navBar =
```

```text
document.querySelector(
```

```text
".ms-compositeHeader-nav"
```

);

Después verifica:

```text
if (navBar && this.properties.Links) {
```

### 11.4. Crear enlaces

Conceptualmente:

```text
this.properties.Links.forEach(
```

link => {

```text
const item =
```

```text
document.createElement("a");
```

item.href = link;

item.innerText =

```text
"Acceso rápido";
```

navBar.appendChild(item);

```text
}
```

);

El ejemplo del módulo muestra esta misma estrategia.

## ACTIVIDAD 12 — Navigation Customizer con Fluent UI

### Objetivo

Construir un menú de aplicaciones utilizando el componente Dropdown de Fluent UI.

### 12.1. Importar

```text
import {
```

Dropdown

```text
} from '@fluentui/react';
```

```text
import * as ReactDom from 'react-dom';
```

### 12.2. Crear opciones

```text
const options = [
```

```text
{
```

key: 'teams',

text: 'Microsoft Teams'

```text
},
```

```text
{
```

key: 'planner',

text: 'Planner'

```text
},
```

```text
{
```

key: 'outlook',

text: 'Outlook'

```text
}
```

];

### 12.3. Crear el Dropdown

```text
const navMenu =
```

```text
document.createElement("div");
```

```text
ReactDom.render(
```

```text
<Dropdown
```

placeholder="Aplicaciones"

options={options}

/>,

navMenu

);

### 12.4. Insertarlo

```text
document.body.insertBefore(
```

navMenu,

```text
document.body.firstChild
```

);

Aplicaciones ▼

Al abrir:

Microsoft Teams

- Planner
- Outlook
## ACTIVIDAD 13 — Integrar SharePoint REST y Graph

Comprobar cómo una extensión puede obtener datos dinámicos de SharePoint y cuándo tendría sentido utilizar Microsoft Graph.

### Paso 1. Probar SharePoint REST

Abre en el navegador la API del sitio de laboratorio, sustituyendo la URL por la de tu tenant:

```text
https://<tenant>.sharepoint.com/sites/PortalProyectos/_api/web/lists/getbytitle('Proyectos')/items
```

Comprueba que la respuesta incluya los elementos de la lista Proyectos.

### Paso 2. Identificar el uso de Microsoft Graph

Identifica qué información adicional podría aportar Graph, por ejemplo usuarios, grupos o equipos.

No agregues permisos de Graph que no sean necesarios para una funcionalidad implementada.

## ACTIVIDAD 14 — Compilar cada extensión

Compila cada solución por separado y corrige cualquier error antes de continuar.

ApplicationCustomizer:

```text
cd C:\SPFx\M4-Extensiones\ApplicationCustomizer
```

```text
heft build
```

FieldCustomizer:

```text
cd C:\SPFx\M4-Extensiones\FieldCustomizer
```

```text
heft build
```

CommandSet:

```text
cd C:\SPFx\M4-Extensiones\CommandSet
```

```text
heft build
```

NavigationCustomizer:

```text
cd C:\SPFx\M4-Extensiones\NavigationCustomizer
```

```text
heft build
```

## ACTIVIDAD 15 — Generar los paquetes

Genera el paquete de producción de cada extensión.

En cada proyecto ejecuta:

```text
heft package-solution --production
```

Localiza el archivo .sppkg en sharepoint/solution/.

Repite el procedimiento para ApplicationCustomizer, FieldCustomizer, CommandSet y NavigationCustomizer.

## ACTIVIDAD 16 — Publicar en App Catalog

### Objetivo

Cargar los cuatro paquetes en el App Catalog de SharePoint.

### Paso 1

Abrir:

SharePoint Admin Center

### Paso 2

Abrir:

App Catalog

### Paso 3

Entrar en:

Apps for SharePoint

### Paso 4

Cargar:

ApplicationCustomizer.sppkg

Después:

FieldCustomizer.sppkg

Después:

CommandSet.sppkg

Y finalmente:

NavigationCustomizer.sppkg

## ACTIVIDAD 17 — Asociar las extensiones al sitio

Verifica que las cuatro soluciones estén disponibles en el sitio Portal de Proyectos y que cada extensión esté asociada a la superficie que debe personalizar.

Abre:

```text
https://<tenant>.sharepoint.com/sites/PortalProyectos
```

Comprueba por separado la página, la lista Proyectos y la navegación.

Si una extensión no aparece, revisa el paquete publicado, su implementación en el App Catalog y la configuración del manifiesto.

## ACTIVIDAD 18 — Prueba integral

Esta es la actividad más importante.

### Prueba 1 — Application Customizer

Abrir:

Portal de Proyectos

Comprueba que aparezca:

Bienvenido al Portal de Proyectos

### Prueba 2 — Navigation Customizer

La navegación debe mostrar:

Aplicaciones ▼

Al abrir:

- Teams
- Planner
- Outlook
### Prueba 3 — Field Customizer

Abrir:

Proyectos

Verificar que Status tenga representación visual.

✓ Completado

◷ En progreso

○ Pendiente

### Prueba 4 — Command Set

Seleccionar un proyecto.

Comprueba que aparezca:

- Acciones del proyecto
con:

- Aprobar
- Rechazar
- Ver detalles
### Prueba 5 — Panel

Seleccionar:

- Ver detalles
Debe abrir:

Detalles del proyecto

con:

Nombre

Propietario

Estado

6. Flujo completo de ejecución

El usuario interactúa con SharePoint Online y cada extensión recibe el contexto de la superficie que personaliza. Application Customizer trabaja sobre la experiencia global; Field Customizer sobre las celdas de una columna; Command Set sobre acciones de elementos; y Navigation Customizer sobre los accesos de navegación. React y Fluent UI pueden utilizarse para la presentación, mientras SharePoint REST y Microsoft Graph aportan datos cuando la funcionalidad lo requiere.

7. ¿Qué sucede técnicamente en cada extensión?

Application Customizer

Página carga

SPFx carga extensión

onInit()

Contexto + propiedades

DOM / React / Fluent UI

Banner o contenido global

Field Customizer

Lista carga

SharePoint identifica columna personalizada

SPFx carga Field Customizer

onRenderCell()

```text
event.fieldValue
```

StatusBadge

DOM de la celda

Este flujo aparece representado explícitamente en el material del módulo.

Command Set

Usuario selecciona elemento

Selecciona comando

onExecute()

```text
event.itemId
```

Lógica específica

Dialog / Panel / API

- Ver detalles
Elemento seleccionado

DetailsPanel

Fluent UI Panel

Información del proyecto

Navigation Customizer

Página carga

onInit()

Contexto de navegación

Links / Dropdown

Menú personalizado

## 8. Casos de uso empresariales

El laboratorio representa escenarios reales.

Application Customizer

Puede utilizarse para:

avisos corporativos;

alertas de mantenimiento;

branding;

accesos globales.

Estos casos están contemplados por el módulo.

Field Customizer

Puede utilizarse para:

estados;

prioridades;

indicadores;

iconos;

validaciones visuales;

acciones rápidas.

Command Set

Puede utilizarse para:

aprobar;

rechazar;

mover;

actualizar;

enviar información;

abrir detalles.

Paneles

Son útiles cuando:

Usuario está en una lista

Necesita más información

No queremos enviarlo a otra página

Abrimos Panel

Navigation Customizer

Puede utilizarse para:

accesos rápidos;

aplicaciones corporativas;

menús personalizados;

integración con Teams;

branding.

## 9. Buenas prácticas que debe aplicar el alumno

### 9.1. No modificar datos para cambiar su presentación

El Field Customizer debe cambiar:

presentación

no:

dato almacenado

Esto es una diferencia fundamental del módulo.

### 9.2. Separar lógica y presentación

Utilizar:

```text
StatusFieldCustomizer.ts
```

StatusBadge.tsx

en lugar de colocar todo en un único archivo.

### 9.3. Limpiar recursos

Siempre considerar:

onDisposeCell()

cuando se utilice React dentro de un Field Customizer.

### 9.4. Utilizar Fluent UI

Cuando exista un componente apropiado, preferir:

MessageBar

Label

Icon

Text

Dropdown

en lugar de construir manualmente todos los elementos HTML.

### 9.5. Agrupar acciones relacionadas

En lugar de:

- Aprobar
- Rechazar
Detalles

como acciones dispersas:

- Acciones del proyecto
- Aprobar
- Rechazar
Detalles

Esto corresponde directamente a la recomendación de agrupación del módulo.

## 10. Problemas frecuentes y solución

### Problema 1 — yo no se reconoce

Verificar:

```text
npm install yo --global
```

y cerrar/reabrir PowerShell.

### Problema 2 — node no se reconoce

Node.js no está instalado o no está en PATH.

Comprobar:

```text
node --version
```

### Problema 3 — La extensión no aparece

Revisar:

App Catalog

Paquete .sppkg

Aplicación desplegada

Sitio correcto

### Problema 4 — Field Customizer no modifica la columna

Comprobar:

Nombre interno de columna

Manifiesto/configuración

Columna correcta

No confundir:

Display Name

con:

Internal Name

### Problema 5 — Command Set no aparece

Comprobar:

Se seleccionó un elemento de la lista.

El comando está configurado para esa lista.

El paquete fue desplegado.

La extensión está instalada.

El manifiesto contiene los IDs correctos.

### Problema 6 — Aparece el comando pero no ocurre nada

Revisar:

```text
event.itemId
```

y comparar contra:

COMMAND_APPROVE

COMMAND_REJECT

COMMAND_DETAILS

### Problema 7 — El panel no recibe información

Verificar:

```text
event.selectedRows[0]
```

y que el objeto se esté pasando a:

DetailsPanel

## 11. Resultado final del laboratorio

El Portal de Proyectos quedará personalizado con un banner global, una representación visual de Status, comandos para gestionar proyectos, un panel de detalles y accesos rápidos de navegación. Las cuatro extensiones se distribuirán mediante paquetes .sppkg y se probarán en SharePoint Online.

## 12. Evidencias del laboratorio

### Evidencia 1 — Ambiente

Captura de:

```text
node --version
```

```text
npm --version
```

```text
yo --version
```

```text
heft --version
```

### Evidencia 2 — Site Collection

Captura de:

Portal de Proyectos

### Evidencia 3 — Lista

Captura de:

Proyectos

con al menos cuatro registros.

### Evidencia 4 — Application Customizer

Captura del:

Banner superior

### Evidencia 5 — Field Customizer

Captura donde Status tenga renderizado personalizado.

### Evidencia 6 — Command Set

Captura del menú:

- Acciones del proyecto
### Evidencia 7 — Panel

Captura de:

Detalles del proyecto

### Evidencia 8 — Navigation Customizer

Captura de:

Aplicaciones ▼

### Evidencia 9 — App Catalog

Captura de los paquetes:

ApplicationCustomizer.sppkg

FieldCustomizer.sppkg

CommandSet.sppkg

NavigationCustomizer.sppkg

## 13. Lista de comprobación final

## 14. Resultado de aprendizaje

Al terminar el laboratorio, podrás distinguir qué superficie de SharePoint modifica cada extensión y elegirla según la necesidad: Web Part para una zona de página, Application Customizer para la experiencia global, Field Customizer para la presentación de una columna, Command Set para acciones sobre elementos y Navigation Customizer para accesos y navegación.

## 15. Resultado integral del laboratorio

El laboratorio integra SPFx, TypeScript, React, Fluent UI y las APIs de SharePoint en una solución que personaliza distintas superficies de SharePoint Online mediante extensiones independientes.

Al finalizar, el alumno habrá pasado de simplemente crear componentes SPFx a extender la interfaz de SharePoint:

La arquitectura final integra SPFx + TypeScript + React + Fluent UI + SharePoint + APIs, que es precisamente la orientación del laboratorio del módulo: personalizar listas, menús y navegación de SharePoint Online mediante extensiones avanzadas.

Referencias técnicas oficiales

Para la preparación del entorno moderno de SPFx, Microsoft documenta Node.js 22 LTS, Heft, Yeoman y el generador de SharePoint.

Configuración del entorno de desarrollo de SharePoint Framework — Microsoft Learn

Extender la interfaz de SharePoint con extensiones SPFx — Microsoft Learn

Application Customizers — Microsoft Learn

Field Customizers — Microsoft Learn

Command Sets — Microsoft Learn

Navigation Customizers — Microsoft Learn

| Tema | Aplicación en el laboratorio |
|---|---|
| 4.1 Application Customizers | Banner global y mensaje corporativo |
| 4.1 Custom Script / DOM | Inserción controlada de elementos |
| 4.1 Contexto y propiedades | Mensajes configurables |
| 4.1 Fluent UI | MessageBar |
| 4.1 SharePoint / Graph | Preparación para datos dinámicos |
| 4.2 Field Customizers | Columna Status |
| 4.2 BaseFieldCustomizer | Clase principal |
| 4.2 onRenderCell() | Renderizado de cada celda |
| 4.2 onDisposeCell() | Liberación de recursos |
| 4.2 React | StatusBadge |
| 4.2 Fluent UI | Label, Icon |
| 4.3 Command Sets | Aprobar, rechazar y ver detalles |
| 4.3 BaseListViewCommandSet | Clase base |
| 4.3 onExecute() | Procesamiento del comando |
| 4.4 Agrupación | Grupo “Acciones del proyecto” |
| 4.4 Panel personalizado | Panel de detalles |
| 4.4 Fluent UI | Panel, Text |
| 4.5 Navigation Customizer | Accesos rápidos |
| 4.5 Propiedades | Lista de enlaces |
| 4.5 Fluent UI | Dropdown |
| 4.5 APIs | Base para navegación dinámica |
| Despliegue | App Catalog + Site Collection |

| Columna | Tipo |
|---|---|
| Title | Texto |
| Owner | Texto |
| Status | Elección |
| Description | Varias líneas |

| Proyecto | Owner | Status |
|---|---|---|
| Portal SPFx | Ana | Completado |
| Migración M365 | Carlos | En progreso |
| Automatización | Miguel | Pendiente |
| Intranet | Laura | Rechazado |

| Criterio | Cumplido |
|---|---|
| Node.js instalado | ☐ |
| npm funcionando | ☐ |
| Visual Studio Code instalado | ☐ |
| Yeoman instalado | ☐ |
| Generador SPFx instalado | ☐ |
| Heft instalado | ☐ |
| Site Collection creado | ☐ |
| Lista Proyectos creada | ☐ |
| Datos de prueba creados | ☐ |
| Application Customizer creado | ☐ |
| BaseApplicationCustomizer utilizado | ☐ |
| onInit() implementado | ☐ |
| Propiedades configurables utilizadas | ☐ |
| Banner superior funcionando | ☐ |
| Footer/mensaje global funcionando | ☐ |
| Fluent UI integrado | ☐ |
| Field Customizer creado | ☐ |
| BaseFieldCustomizer utilizado | ☐ |
| onRenderCell() implementado | ☐ |
| onDisposeCell() implementado | ☐ |
| React utilizado para renderizar celda | ☐ |
| StatusBadge creado | ☐ |
| Fluent UI utilizado en la celda | ☐ |
| Command Set creado | ☐ |
| BaseListViewCommandSet utilizado | ☐ |
| onExecute() implementado | ☐ |
| Comando Aprobar | ☐ |
| Comando Rechazar | ☐ |
| Comando Ver detalles | ☐ |
| Comandos agrupados | ☐ |
| Panel personalizado creado | ☐ |
| Fluent UI Panel utilizado | ☐ |
| Navigation Customizer creado | ☐ |
| Enlaces personalizados funcionando | ☐ |
| Dropdown de aplicaciones funcionando | ☐ |
| SharePoint REST comprendido | ☐ |
| Microsoft Graph identificado como integración | ☐ |
| Soluciones compiladas | ☐ |
| Paquetes .sppkg generados | ☐ |
| Soluciones publicadas en App Catalog | ☐ |
| Extensiones probadas en SharePoint | ☐ |

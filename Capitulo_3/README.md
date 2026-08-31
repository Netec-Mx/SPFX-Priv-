# Laboratorio 3

Panel de proyectos con SPFx, React, Fluent UI y APIs

Duración: 90 minutos

## 1. Objetivo general

Construir un Client-Side Web Part SPFx llamado ProjectDashboard que permita:

Trabajar en un Site Collection de prueba.

Publicar una solución mediante el App Catalog.

Crear un Web Part con React y TypeScript.

Utilizar useState, useEffect y useRef.

Crear y utilizar un hook personalizado.

Utilizar componentes de Fluent UI.

Implementar inputs controlados.

Comprender la relación entre ciclo de vida clásico de React y hooks.

Consultar información de SharePoint mediante REST.

Consultar información del usuario mediante Microsoft Graph.

Manejar los permisos requeridos por Microsoft Graph.

Aplicar validación de entradas.

Aplicar buenas prácticas básicas de rendimiento.

Generar el paquete .sppkg.

Desplegar y validar la solución en SharePoint Online.

Al finalizar, el Web Part mostrará un panel de proyectos con información del usuario, un formulario para agregar proyectos y la lista de proyectos obtenida desde SharePoint.

PANEL DE PROYECTOS

Usuario: <nombre del usuario autenticado>

Nuevo proyecto

Nombre del proyecto Agregar

Proyectos

- Portal SPFx
- Dashboard Viva
- Centro Documental
Contador: 3

## 2. Contexto del laboratorio

En los módulos anteriores el participante preparó el ambiente y conoció SPFx. En este módulo se da el siguiente paso: construir una solución que realmente interactúe con SharePoint y Microsoft 365.

Un Client-Side Web Part se ejecuta en el navegador, se distribuye como parte de un paquete .sppkg y puede consumir Microsoft Graph, REST de SharePoint u otras fuentes.

La solución se ejecutará en una página moderna de SharePoint. El Web Part ProjectDashboard utilizará React para la interfaz, useState, useEffect y useRef para el estado y el ciclo de vida, un hook personalizado para encapsular la lógica de proyectos, SharePoint REST para consultar listas y Microsoft Graph para obtener información del usuario. El resultado se distribuirá mediante un paquete .sppkg publicado en el App Catalog.

SHAREPOINT ONLINE

Site Collection App Catalog

de prueba

.sppkg

Página moderna

ProjectDashboard

React

useState useEffect useRef

Hook personalizado

SharePoint REST Microsoft Graph

Listas Usuario actual

3. Temas del Módulo 3 cubiertos

El objetivo final del laboratorio coincide con el planteamiento del documento: un Web Part que consuma Graph y REST, utilice hooks, un hook personalizado, Fluent UI, inputs controlados y buenas prácticas de seguridad y rendimiento.

## 4. Arquitectura de actividades

## ACTIVIDAD 1

Preparar App Catalog + Site

## ACTIVIDAD 2

Preparar ambiente SPFx

## ACTIVIDAD 3

Crear ProjectDashboard

## ACTIVIDAD 4

Crear el Client-Side Web Part ProjectDashboard con React y TypeScript.

### Paso 1. Crear la carpeta de trabajo

```powershell
mkdir C:\SPFx\Lab3
```

```powershell
cd C:\SPFx\Lab3
```

### Paso 2. Crear el proyecto

Ejecuta:

```powershell
yo @microsoft/sharepoint
```

Selecciona:

Solution name: spfx-lab3

Which type of client-side component: WebPart

What is your Web part name?: ProjectDashboard

Which template would you like to use?: React

Mantén las demás opciones con los valores predeterminados del laboratorio.

### Paso 3. Abrir el proyecto en VS Code

```powershell
cd spfx-lab3
```

```powershell
code .
```

### Paso 4. Identificar la estructura

Localiza src/, config/ y sharepoint/. Dentro de src/webparts/projectDashboard/ identifica ProjectDashboardWebPart.ts, components/ProjectDashboard.tsx y ProjectDashboard.module.scss.

La separación permite distinguir la lógica TypeScript del Web Part, la interfaz React y los estilos.

## ACTIVIDAD 5

Instalar las dependencias del proyecto y comprobar que React y Fluent UI están disponibles.

### Paso 1. Instalar dependencias

Desde la raíz del proyecto:

```powershell
npm install
```

### Paso 2. Verificar Fluent UI

Si el proyecto no contiene @fluentui/react, instala la versión compatible con la solución:

```powershell
npm install @fluentui/react
```

No instales versiones arbitrarias de React. Para SPFx 1.23.2, utiliza React 17.0.1.

## ACTIVIDAD 6

Probar el Web Part localmente antes de desplegarlo en SharePoint.

### Paso 1. Iniciar el entorno de desarrollo

Desde la raíz del proyecto ejecuta:

```powershell
heft start
```

### Paso 2. Abrir la dirección local

Abre la dirección HTTPS local que indique el proceso. El navegador puede mostrar una advertencia sobre el certificado de desarrollo; acéptala únicamente en el ambiente de laboratorio.

### Paso 3. Comprobar el Web Part

Verifica que ProjectDashboard pueda cargarse en el entorno local.

## ACTIVIDAD 7

Input controlado

## ACTIVIDAD 8

REST de SharePoint

## ACTIVIDAD 9

Microsoft Graph

## ACTIVIDAD 10

Ciclo de vida

## ACTIVIDAD 11

Seguridad + rendimiento

## ACTIVIDAD 12

Empaquetar y desplegar

ProjectDashboard funcionando

## ACTIVIDAD 1 — Preparar App Catalog y Site Collection

Incorporar componentes de Fluent UI para construir una interfaz coherente con Microsoft 365.

Preparar el lugar donde se instalará y probará la solución.

El App Catalog funciona como repositorio central de los paquetes .sppkg, mientras que el Site Collection de prueba proporciona un espacio aislado para validar Web Parts sin afectar sitios productivos.

### Paso 1. Acceder al centro de administración

Inicia sesión con una cuenta que tenga permisos administrativos.

Abre:

Microsoft 365 Admin Center SharePoint Admin Center

### Paso 2. Verificar el App Catalog

En el SharePoint Admin Center:

Más características Aplicaciones App Catalog

Si no existe, créalo.

Utiliza, por ejemplo:

Nombre:

App Catalog - Laboratorio SPFx

y una URL apropiada para el tenant.

### Paso 3. Verificar la biblioteca

Dentro del App Catalog localiza:

Apps for SharePoint

Esta biblioteca será el destino del archivo:

projectdashboard.sppkg

### Paso 4. Crear el Site Collection

En:

SharePoint Admin Center Sitios activos Crear

Selecciona:

Sitio de comunicación

Utiliza:

Nombre:

SPFx Lab 3

### Paso 5. Configurar permisos

Asigna:

```powershell
Owner:
```

Member:

Alumno

El laboratorio requiere que el participante pueda:

modificar páginas;

insertar Web Parts;

crear o consultar listas;

probar la solución.

### Paso 6. Crear una página

Dentro de:

SPFx Lab 3

selecciona:

Nuevo Página

Crea:

Panel de proyectos

Todavía no agregaremos nuestro Web Part.

Debe existir:

Tenant

App Catalog

Apps for SharePoint

SPFx Lab 3

Panel de proyectos

## ACTIVIDAD 2 — Crear la lista de proyectos

Crear la fuente de datos que posteriormente consumirá el Web Part mediante REST.

### Paso 1. Crear una lista

En el sitio:

Nuevo Lista Lista en blanco

Nombre:

Proyectos

### Paso 2. Agregar columnas

Crea:

Para Status, utiliza:

Activo

En pausa

Finalizado

### Paso 3. Agregar registros

Introduce:

Nuestro Web Part necesitará datos reales.

La arquitectura será:

Lista Proyectos

SharePoint REST

SPHttpClient

React

ProjectDashboard

## ACTIVIDAD 3 — Preparar el ambiente de desarrollo

### Paso 1. Verificar Node.js

En PowerShell:

```powershell
node --version
```

Debe utilizarse:

Node.js 22 LTS

Para SPFx 1.23.2, la matriz de compatibilidad indica Node.js 22 y React 17.0.1.

### Paso 2. Verificar npm

```powershell
npm --version
```

### Paso 3. Verificar VS Code

```powershell
code --version
```

### Paso 4. Verificar Git

```powershell
git --version
```

### Paso 5. Instalar el CLI indicado por el módulo

```powershell
npm install @microsoft/spfx-cli --global
```

Después:

```powershell
spfx --help
```

El comando de creación planteado por el material es:

```powershell
spfx create --template webpart-react --library-name spfx-lab --component-name ProjectDashboard
```

Microsoft documenta actualmente @microsoft/spfx-cli como el CLI moderno que sustituye al generador Yeoman, pero la documentación todavía lo marca como pre-release. Para producción, Microsoft continúa recomendando el entorno estable correspondiente a la versión de SPFx.

Por ello, para un laboratorio didáctico pueden mantenerse dos rutas:

Ruta A — seguir exactamente el Módulo 3:

```powershell
npm install @microsoft/spfx-cli --global
```

Ruta B — ruta estable de SPFx 1.23.2:

```powershell
npm install -g yo @microsoft/generator-sharepoint
```

y:

```powershell
yo @microsoft/sharepoint
```

La documentación oficial de Microsoft continúa mostrando este mecanismo para crear proyectos SPFx estables.

## ACTIVIDAD 4 — Crear el Client-Side Web Part

### Paso 1. Crear carpeta

```powershell
mkdir C:\SPFx\Lab3
```

```powershell
cd C:\SPFx\Lab3
```

### Paso 2. Crear el proyecto

```powershell
spfx create --template webpart-react --library-name spfx-lab --component-name ProjectDashboard
```

### Paso 3. Abrir VS Code

```powershell
cd spfx-lab
```

```powershell
code .
```

### Paso 4. Identificar la estructura

Localiza:

- src/
webparts/

projectDashboard/

ProjectDashboardWebPart.ts

components/

ProjectDashboard.tsx

ProjectDashboard.module.scss

- config/
- sharepoint/
lógica TypeScript;

vista React;

estilos SCSS.

Esta separación facilita:

mantenimiento;

reutilización;

pruebas;

identificación de errores;

crecimiento del proyecto.

## ACTIVIDAD 5 — Instalar dependencias

Desde la raíz:

```powershell
npm install
```

Si Fluent UI no está disponible en el proyecto, instala la dependencia compatible definida por el proyecto:

```powershell
npm install @fluentui/react
```

No se deben instalar versiones arbitrarias de React.

Para SPFx 1.23.2 la matriz de compatibilidad establece:

SPFx 1.23.2

Node.js 22

React 17.0.1

## ACTIVIDAD 6 — Ejecutar el Web Part en Workbench

El Workbench permite probar el Web Part antes de desplegarlo.

### Paso 1

Ejecuta:

```powershell
npm install
```

### Paso 2

Ejecuta el comando de ejecución indicado por el proyecto:

```powershell
npm run serve
```

```powershell
https://localhost:4321/temp/workbench.html
```

### Paso 3

Abre:

```powershell
https://localhost:4321/temp/workbench.html
```

Agrega:

ProjectDashboard

Todavía no necesitamos desplegar nada en SharePoint.

Primero comprobamos:

Código

Compilación

React

Renderizado

## ACTIVIDAD 7 — Crear el componente React

Abre:

src/webparts/projectDashboard/components/ProjectDashboard.tsx

Utiliza inicialmente:

```powershell
import * as React from 'react';
```

```powershell
export default function ProjectDashboard() {
```

```powershell
return (
```

```powershell
<div>
```

```powershell
<h2>Panel de proyectos</h2>
```

```powershell
<p>Web Part funcionando correctamente.</p>
```

```powershell
</div>
```

);

```powershell
}
```

En Workbench:

Panel de proyectos

Web Part funcionando correctamente.

Que:

SPFx

React

TSX

DOM

están correctamente conectados.

## ACTIVIDAD 8 — Implementar useState

Ahora convertiremos el Web Part en una interfaz dinámica.

Reemplaza el componente por:

```powershell
import * as React from 'react';
```

```powershell
export default function ProjectDashboard() {
```

```powershell
const [count, setCount] = React.useState(0);
```

```powershell
return (
```

```powershell
<div>
```

```powershell
<h2>Panel de proyectos</h2>
```

```powershell
<p>Has hecho clic {count} veces</p>
```

```powershell
<button onClick={() => setCount(count + 1)}>
```

Incrementar

```powershell
</button>
```

```powershell
</div>
```

);

```powershell
}
```

useState(0)

crea:

count

setCount

Cuando ejecutamos:

setCount(count + 1)

React actualiza el estado y vuelve a renderizar.

## ACTIVIDAD 9 — Implementar useEffect

Agrega:

React.useEffect(() => {

console.log(`El contador cambió a ${count}`);

}, [count]);

El componente quedará conceptualmente:

```powershell
const [count, setCount] = React.useState(0);
```

React.useEffect(() => {

console.log(`El contador cambió a ${count}`);

}, [count]);

Probar

Abre las herramientas del navegador.

Selecciona Console.

Pulsa Incrementar.

Observa los mensajes.

useEffect permite ejecutar efectos secundarios.

En SPFx será especialmente importante para:

llamadas a APIs;

carga de datos;

suscripciones;

limpieza de recursos.

## ACTIVIDAD 10 — Implementar useRef

Agrega:

```powershell
const inputRef = React.useRef<HTMLInputElement>(null);
```

Después:

```powershell
const focusInput = () => {
```

inputRef.current?.focus();

};

Y en el JSX:

```powershell
<input
```

ref={inputRef}

placeholder="Escribe algo..."

/>

```powershell
<button onClick={focusInput}>
```

Focalizar input

```powershell
</button>
```

La idea clave de este paso es la siguiente:

useRef permite conservar una referencia hacia un elemento sin provocar un nuevo renderizado al cambiar esa referencia.

El ejemplo del módulo utiliza useRef<HTMLInputElement>(null) para enfocar un campo de texto.

## ACTIVIDAD 11 — Crear un hook personalizado

Esta es una de las actividades más importantes del laboratorio.

### Paso 1. Crear archivo

Dentro de:

components/

crea:

useProjects.ts

### Paso 2. Crear el hook

```powershell
import * as React from 'react';
```

```powershell
export function useProjects() {
```

```powershell
const [projects, setProjects] = React.useState<string[]>([]);
```

React.useEffect(() => {

setProjects([

```powershell
"Portal SPFx",
```

```powershell
"Dashboard Viva",
```

```powershell
"Centro Documental"
```

]);

}, []);

```powershell
return projects;
```

```powershell
}
```

Este patrón está basado en el hook useProjects presentado en el módulo.

### Paso 3. Consumir el hook

En ProjectDashboard.tsx:

```powershell
import { useProjects } from './useProjects';
```

Después:

```powershell
const projects = useProjects();
```

Y:

```powershell
<ul>
```

{projects.map(project => (

```powershell
<li key={project}>
```

{project}

```powershell
</li>
```

))}

```powershell
</ul>
```

¿Por qué crear un hook?

Porque encapsula lógica reutilizable.

En lugar de:

Componente A

carga proyectos

Componente B

carga proyectos

Componente C

carga proyectos

tenemos:

useProjects

/ | \

Componente A

Componente B

Componente C

## ACTIVIDAD 12 — Incorporar Fluent UI

Construir una interfaz consistente con Microsoft 365.

Fluent UI se plantea en el módulo como una arquitectura modular basada en React y TypeScript, con componentes reutilizables, estilos y temas.

### Paso 1. Importar componentes

En ProjectDashboard.tsx:

```powershell
import {
```

Stack,

TextField,

PrimaryButton

} from '@fluentui/react';

### Paso 2. Crear la estructura

```powershell
<Stack tokens={{ childrenGap: 10 }}>
```

```powershell
<h2>Panel de proyectos</h2>
```

```powershell
<TextField
```

label="Nombre del proyecto"

/>

```powershell
<PrimaryButton
```

text="Agregar proyecto"

/>

```powershell
</Stack>
```

En lugar de crear todos los controles desde cero:

HTML

CSS

JavaScript

utilizamos componentes reutilizables:

TextField

Button

Stack

## ACTIVIDAD 13 — Crear un input controlado

En un input controlado, React mantiene el valor del campo como parte del estado del componente.

Crear un input controlado cuyo valor permanezca sincronizado con el estado de React.

Ahora conectaremos el formulario con React.

### Paso 1. Crear estado

```powershell
const [projectName, setProjectName] =
```

React.useState('');

### Paso 2. Conectar el TextField

```powershell
<TextField
```

label="Nombre del proyecto"

value={projectName}

onChange={(_, value) =>

setProjectName(value || '')

```powershell
}
```

/>

### Paso 3. Mostrar el valor

Agrega:

```powershell
<p>
```

Proyecto:

{projectName}

```powershell
</p>
```

¿Qué significa "controlado"?

El flujo ahora es:

Usuario escribe

onChange

setProjectName()

React actualiza estado

Render

TextField muestra nuevo valor

## ACTIVIDAD 14 — Validar y transformar la entrada

Validar antes de actualizar el estado evita propagar valores vacíos o normalizables al resto de la interfaz.

Validar y normalizar la entrada antes de actualizar el estado del formulario.

Modifica el evento:

onChange={(_, value) =>

setProjectName(

(value || '').trim()

)

```powershell
}
```

También podemos normalizar:

setProjectName(

(value || '').trim().toUpperCase()

)

La validación y transformación temprana evita almacenar datos innecesariamente inconsistentes.

## ACTIVIDAD 15 — Conectar con SharePoint REST

SPHttpClient permite realizar la consulta autenticada hacia SharePoint desde el contexto de la solución.

Consultar la lista Proyectos de SharePoint mediante SharePoint REST.

Ahora el laboratorio deja de ser una simulación.

Vamos a leer la lista:

Proyectos

### Paso 1. Importar SPHttpClient

En el Web Part principal:

```powershell
import { SPHttpClient } from '@microsoft/sp-http';
```

### Paso 2. Obtener la información

El contexto de SPFx proporciona:

this.context.spHttpClient

### Paso 3. Crear una función

En el Web Part:

```powershell
private async getProjects(): Promise<any[]> {
```

```powershell
const url =
```

`${this.context.pageContext.web.absoluteUrl}` +

`/_api/web/lists/getbytitle('Proyectos')/items`;

```powershell
const response =
```

await this.context.spHttpClient.get(

url,

SPHttpClient.configurations.v1

);

```powershell
const data = await response.json();
```

```powershell
return data.value;
```

```powershell
}
```

¿Qué hace?

Construye:

```powershell
https://tenant.sharepoint.com/sites/SPFxLab3
```

/_api/web/lists/getbytitle('Proyectos')/items

SharePoint devuelve:

{

```powershell
"value": [
```

{

```powershell
"Title": "Portal SPFx",
```

```powershell
"Owner": "Miguel"
```

```powershell
}
```

]

```powershell
}
```

## ACTIVIDAD 16 — Pasar datos desde SPFx hacia React

Las props separan responsabilidades: SPFx obtiene los datos y React decide cómo presentarlos.

Pasar los datos obtenidos por SPFx al componente React mediante props y tipos TypeScript.

Aquí aparece un concepto fundamental.

SPFx Web Part

obtiene datos

SharePoint REST

Web Part

props

React

ProjectDashboard

El componente React debe recibir los proyectos.

Por ejemplo:

```powershell
export interface IProject {
```

Id: number;

```powershell
Title: string;
```

```powershell
Owner: string;
```

```powershell
Status: string;
```

```powershell
}
```

Y:

```powershell
export interface IProjectDashboardProps {
```

projects: IProject[];

```powershell
}
```

Después:

```powershell
export default function ProjectDashboard(
```

props: IProjectDashboardProps

) {

Y renderizamos:

{props.projects.map(project => (

```powershell
<li key={project.Id}>
```

{project.Title} — {project.Owner}

```powershell
</li>
```

))}

## ACTIVIDAD 17 — Conectar Microsoft Graph

Graph permite obtener información del usuario autenticado que no forma parte de la lista Proyectos.

Consultar Microsoft Graph para obtener información del usuario autenticado.

Ahora consultaremos información de Microsoft 365.

Microsoft Graph

Microsoft 365

de:

SharePoint REST

Listas / sitios / campos / búsqueda

### Paso 1. Importar MSGraphClientV3

```powershell
import { MSGraphClientV3 } from '@microsoft/sp-http';
```

### Paso 2. Obtener el cliente

```powershell
const client: MSGraphClientV3 =
```

await this.context.msGraphClientFactory

.getClient('3');

### Paso 3. Consultar al usuario

```powershell
const response =
```

await client.api('/me').get();

### Paso 4. Mostrar el nombre

console.log(response.displayName);

## ACTIVIDAD 18 — Configurar permisos de Microsoft Graph

Los permisos deben corresponder con las operaciones que realmente ejecuta el Web Part y formar parte de la configuración de la solución.

Configurar los permisos de Microsoft Graph necesarios para la funcionalidad del Web Part.

Este paso es crítico.

Un Web Part no debe pedir permisos innecesarios.

En:

config/package-solution.json

localiza:

```powershell
"webApiPermissionRequests": []
```

Configura:

```powershell
"webApiPermissionRequests": [
```

{

```powershell
"resource": "Microsoft Graph",
```

```powershell
"scope": "User.Read"
```

```powershell
}
```

]

El flujo es:

Web Part

Declara permiso

App Catalog

Administrador

Aprueba permiso

Microsoft Graph

No debemos solicitar:

Directory.ReadWrite.All

si solamente necesitamos:

User.Read

## ACTIVIDAD 19 — Implementar el ciclo de vida

Los hooks permiten expresar en un componente funcional comportamientos que anteriormente se asociaban al ciclo de vida de componentes de clase.

Comprender y aplicar el ciclo de vida del componente mediante hooks de React.

React clásico

componentDidMount()

componentDidUpdate()

componentWillUnmount()

React moderno

useEffect()

useState()

### Paso 1. Comprender componentDidMount

En una clase:

componentDidMount() {

console.log('Componente montado');

```powershell
}
```

Se ejecuta una vez después del montaje.

### Paso 2. Comprender componentDidUpdate

componentDidUpdate(

prevProps,

prevState

) {

if (prevState.count !== this.state.count) {

console.log('El contador cambió');

```powershell
}
```

```powershell
}
```

### Paso 3. Equivalencia moderna

Para un componente funcional:

React.useEffect(() => {

console.log('Componente montado');

}, []);

equivale conceptualmente al montaje.

Y:

React.useEffect(() => {

console.log(`Cambio: ${count}`);

}, [count]);

representa el efecto asociado al cambio de count.

### Paso 4. Aplicarlo al Web Part

Utiliza:

React.useEffect(() => {

console.log('ProjectDashboard montado');

```powershell
return () => {
```

console.log('ProjectDashboard desmontado');

};

}, []);

¿Por qué existe return?

Para limpiar:

listeners;

timers;

suscripciones;

recursos temporales.

Esto permite evitar fugas de memoria y comportamientos inesperados.

## ACTIVIDAD 20 — Convertir useProjects en un hook real

Convertir useProjects en un hook que gestione la carga de datos desde SharePoint sin mezclar la lógica de acceso con la presentación.

### Paso 1. Definir el hook

Crea o modifica components/useProjects.ts:

```powershell
import * as React from 'react';
```

```powershell
import { IProject } from './IProject';
```

```powershell
export function useProjects(loadProjects: () => Promise<IProject[]>) {
```

```powershell
const [projects, setProjects] = React.useState<IProject[]>([]);
```

React.useEffect(() => {

let cancelled = false;

loadProjects().then(data => {

if (!cancelled) setProjects(data);

});

```powershell
return () => { cancelled = true; };
```

}, [loadProjects]);

```powershell
return projects;
```

```powershell
}
```

### Paso 2. Proporcionar una función estable desde el componente padre

Utiliza React.useCallback para evitar que la función cambie en cada renderizado:

```powershell
const loadProjects = React.useCallback(() => getProjects(), []);
```

Después:

```powershell
const projects = useProjects(loadProjects);
```

El hook encapsula la carga de datos y el componente se concentra en presentar el resultado.

## ACTIVIDAD 21 — Construir el formulario completo

El formulario reúne en una misma experiencia los conceptos trabajados por separado en las actividades anteriores.

Integrar estado, validación, Fluent UI y datos externos en el formulario completo.

El formulario tendrá:

Nombre

Owner

Status

Con Fluent UI:

```powershell
<Stack tokens={{ childrenGap: 10 }}>
```

```powershell
<TextField
```

label="Nombre del proyecto"

value={projectName}

onChange={(_, value) =>

setProjectName(value || '')

```powershell
}
```

/>

```powershell
<TextField
```

label="Responsable"

value={owner}

onChange={(_, value) =>

setOwner(value || '')

```powershell
}
```

/>

```powershell
<PrimaryButton
```

text="Agregar proyecto"

onClick={handleAddProject}

/>

```powershell
</Stack>
```

Validación

Antes de procesar:

if (!projectName.trim()) {

return;

```powershell
}
```

Después:

if (!owner.trim()) {

return;

```powershell
}
```

Nunca debemos asumir que la entrada del usuario es válida.

## ACTIVIDAD 22 — Seguridad de la información

La seguridad incluye tanto los permisos solicitados como la validación de entradas y la cantidad de información que se expone.

Aplicar controles básicos de seguridad relacionados con permisos, validación y exposición de datos.

Revisaremos cuatro reglas.

### Regla 1 — Mínimo privilegio

Solicitar:

User.Read

y no permisos más amplios si no son necesarios.

### Regla 2 — No guardar tokens manualmente

No hacer:

localStorage.setItem(

```powershell
"accessToken",
```

token

);

### Regla 3 — Validar entradas

Antes de mostrar información proveniente de fuentes externas, validar y sanitizar cuando corresponda.

### Regla 4 — HTTPS

Las llamadas externas deben utilizar HTTPS.

## ACTIVIDAD 23 — Optimización de rendimiento

Aplicar optimizaciones básicas para reducir llamadas innecesarias y mejorar el rendimiento.

Aplicaremos tres conceptos del módulo.

1. Evitar llamadas innecesarias

No ejecutar una API cada vez que ocurre un render.

Incorrecto conceptualmente:

render

API

render

API

render

API

Usaremos useEffect con dependencias apropiadas.

2. Ejecutar llamadas en paralelo

Cuando dos consultas son independientes:

```powershell
const [users, projects] =
```

await Promise.all([

getUsers(),

getProjects()

]);

3. Lazy loading

Como ejercicio conceptual:

```powershell
const DetalleProyecto =
```

React.lazy(

() => import('./DetalleProyecto')

);

Esto permite cargar componentes solamente cuando son necesarios.

## ACTIVIDAD 24 — Revisar TypeScript estricto

Revisar el uso de TypeScript estricto para detectar incompatibilidades antes de ejecutar la solución.

Abre:

tsconfig.json

Revisa la configuración de TypeScript.

```powershell
"strict": true
```

cuando corresponda al proyecto, porque permite detectar errores durante la compilación.

## ACTIVIDAD 25 — Compilar la solución

Compilar la solución mediante Heft y comprobar que TypeScript, React, Fluent UI y SPFx pueden procesarse como una única solución.

### Paso 1. Compilar

Desde la raíz:

```powershell
heft build
```

La compilación debe terminar sin errores.

Si aparecen errores, corrígelos antes de continuar con el empaquetado.

## ACTIVIDAD 26 — Generar el paquete .sppkg

Generar el paquete de producción que se instalará en SharePoint.

### Paso 1. Generar el paquete

Desde la raíz:

```powershell
heft package-solution --production
```

### Paso 2. Localizar el artefacto

Busca el archivo .sppkg en sharepoint/solution/.

Ese archivo será el que publicarás en el App Catalog.

## ACTIVIDAD 27 — Publicar en el App Catalog

Publicar el paquete .sppkg en el App Catalog y completar la implementación de la solución.

### Paso 1

Abre:

App Catalog

### Paso 2

En:

Apps for SharePoint

carga:

projectdashboard.sppkg

### Paso 3

Implementa la aplicación cuando SharePoint lo solicite.

### Paso 4

Si la solución solicita el permiso Microsoft Graph User.Read, la solicitud deberá ser aprobada por un administrador.

Microsoft Graph

User.Read

el administrador deberá aprobarla.

## ACTIVIDAD 28 — Instalar en el Site Collection

La instalación permite agregar el Web Part a una página y comprobarlo en un contexto de SharePoint real.

Instalar la solución en el Site Collection de prueba y agregar el Web Part a una página.

Regresa a:

SPFx Lab 3

Abre:

Panel de proyectos

Selecciona:

Editar

Después:

+

Busca:

ProjectDashboard

Agrégalo.

## ACTIVIDAD 29 — Validar el Web Part

Realiza las siguientes pruebas.

### Prueba 1 — Renderizado

Debe aparecer:

Panel de proyectos

### Prueba 2 — React

Pulsa:

Incrementar

El contador debe aumentar.

### Prueba 3 — Input controlado

Escribe:

Nuevo proyecto

El estado debe actualizarse inmediatamente.

### Prueba 4 — REST

La lista debe mostrar:

Portal SPFx

Dashboard Viva

Centro Documental

### Prueba 5 — Graph

Debe aparecer el usuario autenticado.

Por ejemplo:

Usuario: <nombre del usuario autenticado>

### Prueba 6 — Fluent UI

Los controles deben utilizar:

TextField

PrimaryButton

Stack

## 5. Flujo técnico final

El recorrido completo es: la página moderna de SharePoint aloja ProjectDashboard; SPFx proporciona el contexto y los clientes autenticados; los hooks administran estado, efectos y referencias; SharePoint REST y Microsoft Graph proporcionan datos; React renderiza la información mediante componentes de Fluent UI.

## 6. Relación entre los hooks y el Web Part

## 7. Relación entre React clásico y React moderno

El alumno debe poder interpretar código antiguo y código moderno.

Esta equivalencia forma parte explícita del contenido del Módulo 3.

## 8. Resultado final del laboratorio

Al finalizar, tendrás un Web Part ProjectDashboard construido con React, TypeScript y Fluent UI, con estado mediante hooks, formulario controlado, carga de proyectos desde SharePoint REST, información del usuario mediante Microsoft Graph, permisos configurados, validaciones básicas, optimizaciones y un paquete .sppkg desplegado en SharePoint.

## 9. Evidencias del laboratorio

### Evidencia 1 — Ambiente

Captura donde se vea:

```powershell
node --version
```

```powershell
npm --version
```

```powershell
spfx --help
```

```powershell
git --version
```

### Evidencia 2 — Proyecto

Captura de VS Code mostrando:

- src/
- config/
- sharepoint/
### Evidencia 3 — React

Web Part funcionando con:

useState

useEffect

useRef

### Evidencia 4 — Fluent UI

Web Part mostrando:

TextField

PrimaryButton

Stack

### Evidencia 5 — SharePoint REST

Proyectos cargados desde:

Lista Proyectos

### Evidencia 6 — Microsoft Graph

Nombre del usuario obtenido mediante:

/me

### Evidencia 7 — App Catalog

Archivo:

*.sppkg

visible en:

Apps for SharePoint

### Evidencia 8 — Resultado final

Página de SharePoint con:

PANEL DE PROYECTOS

Usuario: __________

Proyecto: [____________________]

Responsable: [__________________]

[ Agregar proyecto ]

Proyectos

- Portal SPFx
- Dashboard Viva
- Centro Documental
## 10. Lista de comprobación final

## 11. Resultado de aprendizaje esperado

Al terminar el laboratorio, podrás explicar y ejecutar el ciclo completo: preparar el entorno, crear el Web Part, construir la interfaz, administrar estado con React, consumir SharePoint REST y Microsoft Graph, validar entradas, aplicar mínimo privilegio y optimizar llamadas, empaquetar la solución, publicarla en el App Catalog y probarla en SharePoint Online.

| Tema del módulo | Actividad del laboratorio |
|---|---|
| 3.1 App Catalog y Site Collection | Preparación del tenant |
| 3.2 Ambiente SPFx | Verificación e instalación |
| 3.3 Client-Side Web Parts | Creación de ProjectDashboard |
| 3.4 React dinámico | Componente React |
| 3.5 Hooks | useState, useEffect, useRef, hook personalizado |
| 3.6 Fluent UI | TextField, PrimaryButton, Stack |
| 3.7 Ciclo de vida | Comparación clase vs hooks |
| 3.8 Inputs controlados | Formulario de nuevo proyecto |
| 3.9 Preparación para React | Componentes funcionales, TypeScript, hooks y arquitectura |
| 3.10 Graph y REST | Usuario + proyectos |
| 3.11 Seguridad y rendimiento | Validación, mínimo privilegio, caché/optimización y lazy loading |

| Columna | Tipo |
|---|---|
| Title | Texto |
| Owner | Texto |
| Status | Elección |
| Description | Varias líneas |

| Title | Owner | Status |
|---|---|---|
| Portal SPFx | Miguel | Activo |
| Dashboard Viva | Ana | Activo |
| Centro Documental | Carlos | En pausa |

| Hook | Uso dentro del laboratorio |
|---|---|
| useState | Contador, formulario y datos |
| useEffect | Carga de proyectos y efectos |
| useRef | Referencia al input |
| Hook personalizado | Encapsular carga de proyectos |

| Concepto | React clásico | React moderno |
|---|---|---|
| Estado | this.state | useState() |
| Actualizar estado | setState() | setState() retornado por hook |
| Montaje | componentDidMount() | useEffect(..., []) |
| Actualización | componentDidUpdate() | useEffect(..., [dependencia]) |
| Desmontaje | componentWillUnmount() | cleanup de useEffect() |

| Criterio | Cumplido |
|---|---|
| App Catalog disponible | ☐ |
| Site Collection de prueba creado | ☐ |
| Lista Proyectos creada | ☐ |
| Ambiente Node/VS Code preparado | ☐ |
| Proyecto SPFx creado | ☐ |
| Client-Side Web Part funcionando | ☐ |
| React funcionando | ☐ |
| useState implementado | ☐ |
| useEffect implementado | ☐ |
| useRef implementado | ☐ |
| Hook personalizado implementado | ☐ |
| Fluent UI implementado | ☐ |
| Input controlado implementado | ☐ |
| Validación de entrada implementada | ☐ |
| Ciclo de vida comprendido | ☐ |
| SharePoint REST funcionando | ☐ |
| Microsoft Graph funcionando | ☐ |
| User.Read solicitado correctamente | ☐ |
| Mínimo privilegio aplicado | ☐ |
| Buenas prácticas de rendimiento aplicadas | ☐ |
| Solución compilada | ☐ |
| .sppkg generado | ☐ |
| Solución publicada en App Catalog | ☐ |
| Web Part instalado en SharePoint | ☐ |
| Validación final realizada | ☐ |

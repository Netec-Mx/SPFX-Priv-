# Laboratorio 3 --- Panel de proyectos con SPFx, React, Fluent UI y APIs

**Duración estimada:** 180 minutos

Duración estimada: 180 minutos

## Objetivo

Construir un Client-Side Web Part SPFx denominado `ProjectDashboardXXX`
que utilice React y TypeScript para presentar proyectos de una lista de
SharePoint, mostrar información del usuario autenticado mediante
`Microsoft Graph` y proporcionar un formulario con controles de Fluent
UI. Durante el desarrollo se aplicarán `useState`, `useEffect`,
`useRef`, un hook personalizado, inputs controlados, validación,
separación de responsabilidades y prácticas básicas de seguridad y
rendimiento.

## Alcance

El laboratorio cubre desde la preparación del Site Collection y la lista
de datos hasta la compilación, generación del paquete .sppkg,
publicación en el App Catalog, aprobación del permiso `User.Read` y
validación del Web Part en una página moderna de SharePoint Online. El
laboratorio utiliza la ruta estable de creación de proyectos SPFx
mediante Yeoman y el generador de SharePoint.

## Requisitos

Cuenta de Microsoft 365 con acceso a un tenant de laboratorio de
SharePoint Online.

Acceso al sitio `` Portal-`Proyectos`XXX `` con permisos para editar
páginas y crear listas. El App Catalog compartido y la aprobación de
permisos de API son administrados por el instructor o el administrador
del tenant.

Node.js 22.23.2 instalado y disponible en PowerShell.

Git instalado.

Visual Studio Code instalado.

PowerShell 7 para las tareas de PnP PowerShell, si se utiliza esa
herramienta en el entorno del curso.

SPFx 1.23.2 y React 17.0.1 para el proyecto de este laboratorio.

Conexión a Internet para descargar dependencias y acceder a Microsoft
365.

## Antes de comenzar

Este laboratorio utiliza SPFx 1.23.2. Para mantener un procedimiento
reproducible, se utilizará explícitamente el generador
@microsoft/generator-sharepoint 1.23.2 y la opción SharePoint Online
only (latest) del asistente. El App Catalog es compartido y su
administración corresponde al instructor. Cada participante trabajará en
su sitio `` Portal-`Proyectos`XXX `` y utilizará nombres únicos para la
solución y el Web Part. No se utilizará el CLI de SPFx en este
laboratorio.

## Actividad 1. Preparar el App Catalog y el sitio de laboratorio

Preparar el destino de publicación y el sitio de SharePoint donde se
probará el Web Part.

### Paso 1. Confirmar el App Catalog compartido

Inicia sesión en Microsoft 365 con una cuenta que tenga permisos
administrativos. Abre el Centro de administración de SharePoint. En la
interfaz en español, selecciona Centros de administración \> SharePoint.

### Paso 2. Verificar el App Catalog

El instructor administra el App Catalog compartido del tenant. No crees
un App Catalog nuevo ni modifiques su configuración. Confirma con el
instructor que el catálogo está disponible y que se utilizará para
publicar el paquete `.sppkg` del laboratorio.

**Resultado esperado.** El instructor confirma que el App Catalog
compartido está disponible y que la biblioteca `Apps for SharePoint`
será el destino de publicación.

### Paso 3. Crear el sitio del participante

El instructor o el administrador del tenant crea un sitio de
comunicación para tu práctica con el nombre exacto
`` Portal-`Proyectos`XXX ``, donde `XXX` representa tus iniciales. Si el
sitio ya fue creado, utilízalo y no crees otro.

### Paso 4. Confirmar permisos en el sitio

Abre `` Portal-`Proyectos`XXX ``. Tu cuenta debe poder editar páginas y
crear listas en ese sitio. Si no tienes esos permisos, solicita al
instructor o al administrador del sitio que te los asigne antes de
continuar.

### Paso 5. Crear la página de prueba

Dentro de `` Portal-`Proyectos`XXX ``, selecciona Nuevo \> Página.
Asigna el título `Panel de proyectos` y publica la página. No agregues
todavía el Web Part.

**Resultado esperado.** Existe un App Catalog disponible y existe el
sitio `Portal-ProyectosXXX` con una página publicada llamada
`Panel de proyectos`.

## Actividad 2. Crear la lista de proyectos

Crear la fuente de datos que el Web Part consultará mediante SharePoint
REST.

### Paso 1. Crear la lista

En `` Portal-`Proyectos`XXX ``, selecciona Nuevo \> Lista \> Lista en
blanco. Escribe `Proyectos` como nombre y selecciona Crear.

### Paso 2. Crear la columna Owner

Abre la lista `Proyectos`. Selecciona + Agregar columna \> Una línea de
texto. Escribe `Owner` como nombre de columna y guarda.

### Paso 3. Crear la columna Status

Selecciona + Agregar columna \> Elección. Escribe `Status` como nombre.
Agrega exactamente estas opciones: `Activo`, `En pausa`, `Finalizado`.
Guarda la columna.

### Paso 4. Crear la columna Description

Selecciona + Agregar columna \> Varias líneas de texto. Escribe
`Description` como nombre y guarda.

### Paso 5. Agregar registros

Agrega tres elementos con estos valores:

  -----------------------------------------------------------------------
  Title             Owner             Status            Description
  ----------------- ----------------- ----------------- -----------------
  Portal SPFx       Miguel            Activo            Portal para
                                                        soluciones SPFx

  Dashboard Viva    Ana               Activo            Panel para
                                                        información de
                                                        Microsoft 365

  Centro Documental Carlos            En pausa          Gestión
                                                        documental
  -----------------------------------------------------------------------

**Resultado esperado.** La lista `Proyectos` contiene tres registros y
las columnas `Title`, `Owner`, `Status` y `Description`.

## Actividad 3. Verificar el ambiente de desarrollo

Comprobar las versiones que utilizará el proyecto antes de crear la
solución.

### Paso 1. Abrir PowerShell

Abre PowerShell 7. No utilices el símbolo del sistema clásico para las
instrucciones que indiquen PowerShell.

**Comandos de verificación**

  -----------------------------------------------------------------------
  node --version`<br>`{=html}npm --version`<br>`{=html}where.exe
  node`<br>`{=html}where.exe npm`<br>`{=html}git
  --version`<br>`{=html}code --version`<br>`{=html}yo
  --version`<br>`{=html}npm list -g @microsoft/generator-sharepoint
  --depth=0`<br>`{=html}npm list -g @rushstack/heft --depth=0
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

**Resultado esperado.** `node --version` muestra `v22.23.2` y
`npm --version` muestra `10.9.8`. Git y VS Code responden con una
versión instalada. Si Node.js no muestra `v22.23.2`, detén el
laboratorio y corrige la versión antes de crear el proyecto.

El bloque anterior verifica también que el generador sea exactamente
`@microsoft/generator-sharepoint@1.23.2` y que Heft sea `1.2.25`.

### Paso 2. Confirmar las versiones globales

Revisa las dos últimas líneas del bloque anterior para confirmar el
generador y Heft.

**Resultado esperado.** Yeoman responde con su versión instalada y las
consultas `npm list -g` identifican
`@microsoft/generator-sharepoint@1.23.2` y `@rushstack/heft@1.2.25`.

## Actividad 4. Crear el proyecto SPFx

Crear el Web Part `ProjectDashboardXXX` con React y TypeScript
utilizando el generador estable.

### Paso 1. Crear la carpeta del proyecto

**Ejecuta:**

  -----------------------------------------------------------------------
  New-Item -ItemType Directory -Path
  C:`\SPFx`{=tex}`\spfx`{=tex}-lab3-webpart-XXX
  -Force`<br>`{=html}Set-Location
  C:`\SPFx`{=tex}`\spfx`{=tex}-lab3-webpart-XXX
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

### Paso 2. Ejecutar el generador

**Ejecuta:**

  yo @microsoft/sharepoint
  --------------------------

### Paso 3. Responder el asistente

Responde el asistente con estos valores. En SPFx 1.23.2 no se muestra la
antigua pregunta interactiva sobre despliegue tenant-wide; esa
configuración se controlará explícitamente en
`config/`package-solution.json\`\` en el siguiente paso.

Solution name: `spfx-lab3-webpart-XXX`

Which baseline packages do you want to target for your component(s)?:
`SharePoint Online only (latest)`

Where do you want to place the files?: `Use the current folder`

Which type of client-side component to create?: `WebPart`

What is your Web part name?: `ProjectDashboardXXX`

Which template would you like to use?: `React`

En SPFx 1.23.2, después de seleccionar `React` no se requieren
respuestas adicionales en este laboratorio. No modifiques manualmente el
proyecto antes de completar el paso siguiente.

### Paso 4. Configurar el despliegue para el tenant compartido

Abre
`config/`package-solution.json`` . Dentro de la propiedad `solution`, agrega o verifica la propiedad ``skipFeatureDeployment\``con el valor`false\`.

  "skipFeatureDeployment": false
  --------------------------------

**Resultado esperado.** La carpeta `C:\SPFx\spfx-lab3-webpart-XXX`
contiene el proyecto y `package.json`. El archivo
`config/package-solution.json` conserva `skipFeatureDeployment` con
valor `false`.

## Actividad 5. Instalar dependencias y preparar el certificado de desarrollo

Instalar las dependencias locales del proyecto y confiar en el
certificado HTTPS utilizado por el entorno de desarrollo.

### Paso 1. Abrir el proyecto

Desde `C:\SPFx\`spfx-lab3-webpart-XXX\`\`, ejecuta:

  code .
  --------

### Paso 2. Instalar dependencias

En la terminal integrada de VS Code, ubicada en la raíz del proyecto,
ejecuta:

  npm install
  -------------

### Paso 3. Verificar React y Fluent UI

**Ejecuta:**

  npm list react react-dom @fluentui/react --depth=0
  ----------------------------------------------------

**Resultado esperado.** React aparece como `17.0.1`. Si
`@fluentui/react` no aparece, instala la dependencia con el siguiente
comando y vuelve a ejecutar la verificación.

  npm install @fluentui/react
  -----------------------------

### Paso 4. Confiar en el certificado de desarrollo

Desde la raíz del proyecto ejecuta:

  heft trust-dev-cert
  ---------------------

**Resultado esperado.** Heft completa el proceso de confianza del
certificado de desarrollo. Esta operación se realiza una vez por
estación de trabajo.

## Actividad 6. Reconocer la estructura del proyecto

Identificar los archivos que contienen la configuración, la lógica del
Web Part, el componente React y los estilos.

### Paso 1. Abrir el Explorador de VS Code

En el Explorador de archivos de VS Code, expande la carpeta del
proyecto.

### Paso 2. Identificar las carpetas

Localiza exactamente estas carpetas: `src`, `config` y `sharepoint`. Si
`sharepoint` todavía no aparece, no la crees manualmente; aparecerá
cuando el proceso de empaquetado genere la salida de la solución.

### Paso 3. Identificar los archivos del Web Part

Dentro de `src/webparts/projectDashboardXXX/`, localiza
`` ProjectDashboardXXX`WebPart.ts`. Dentro de `components/`, localiza ``ProjectDashboardXXX`.tsx`
y \``ProjectDashboardXXX`.module.scss\`.

### Paso 4. Identificar la configuración

En la raíz localiza `package.json` y `tsconfig.json`. En `config`,
localiza `rig.json`, `package-solution.json` y `serve.json`.

**Resultado esperado.** Puedes distinguir el código del Web Part, el
componente React, los estilos, las dependencias y los archivos de
configuración. No se asume que `sharepoint/solution` exista antes del
empaquetado.

## Actividad 7. Ejecutar el Web Part en SharePoint

Comprobar que el proyecto generado puede ejecutarse antes de modificar
el código.

### Paso 1. Definir el sitio utilizado por el Hosted Workbench

En PowerShell, desde la raíz del proyecto, asigna el dominio y la ruta
del sitio de laboratorio a la variable utilizada por SPFx. No incluyas
`https://` porque `serve.json` ya contiene el protocolo.

  -----------------------------------------------------------------------
  \$env:SPFX_SERVE_TENANT_DOMAIN =
  "`<tenant>`{=html}.sharepoint.com/sites/Portal-ProyectosXXX"
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

### Paso 2. Iniciar Heft

**Ejecuta:**

  heft start
  ------------

### Paso 3. Abrir el Hosted Workbench

Heft iniciará el servidor de desarrollo y mostrará la dirección del
entorno de prueba. Abre la dirección indicada por la salida del comando.
El Hosted Workbench de SharePoint Online está en transición: Microsoft
lo marcó como obsoleto desde mayo de 2026 y anunció su retiro para el 1
de diciembre de 2026. Mientras el entorno del curso continúe
disponiéndolo, este paso permite probar el proyecto con el contexto de
SharePoint. Si el tenant ya utiliza el Debug Toolbar, sigue el mecanismo
indicado por el instructor.

El Hosted Workbench de SharePoint Online está en transición: Microsoft
lo marcó como obsoleto desde mayo de 2026 y anunció su retiro para el 1
de diciembre de 2026. Mientras el entorno del curso continúe
disponiéndolo, este paso permite probar el proyecto con el contexto de
SharePoint. Para entornos que ya hayan migrado al Debug Toolbar, se debe
utilizar ese mecanismo de depuración.

**Resultado esperado.** El proyecto inicia sin errores y el Web Part
`ProjectDashboardXXX` puede cargarse en el entorno de prueba.

## Actividad 8. Crear el modelo de proyecto en TypeScript

Definir un tipo explícito para los elementos que llegan desde
SharePoint.

### Paso 1. Crear el archivo IProject.ts

En
\``src/webparts/projectDashboardXXX/`components/`, crea un archivo llamado`IProject.ts\`.

**Archivo: src/webparts/projectDashboardXXX/components/IProject.ts**

  -----------------------------------------------------------------------
  export interface IProject {`<br>`{=html} Id: number;`<br>`{=html}
  Title: string;`<br>`{=html} Owner: string;`<br>`{=html} Status:
  string;`<br>`{=html} Description: string;`<br>`{=html}}
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

### Paso 2. Comprobar el tipado

En el mismo archivo, no agregues propiedades distintas a las definidas.
El tipo se utilizará para evitar que el componente React dependa de
objetos sin estructura conocida.

**Resultado esperado.** Existe `IProject.ts` con cinco propiedades
tipadas.

## Actividad 9. Implementar useState

Agregar estado React para el contador y comprobar la actualización de la
interfaz.

### Paso 1. Reemplazar el componente React

Abre el archivo indicado y reemplaza su contenido completo.

**Archivo:
src/webparts/projectDashboardXXX/components/ProjectDashboardXXX.tsx**

  -----------------------------------------------------------------------
  import \* as React from 'react';`<br>`{=html}`<br>`{=html}export
  default function ProjectDashboardXXX(): React.ReactElement
  {`<br>`{=html} const \[count, setCount\] =
  React.useState`<number>`{=html}(0);`<br>`{=html}`<br>`{=html} return
  (`<br>`{=html}
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

### Paso 2. Guardar y observar el resultado

Guarda el archivo. Si `heft start` continúa ejecutándose, el navegador
debe actualizar el Web Part. Pulsa `Incrementar` dos veces.

**Resultado esperado.** El contador cambia de 0 a 1 y después a 2 sin
recargar la página.

## Actividad 10. Implementar useEffect

Ejecutar un efecto cada vez que cambie el contador.

### Paso 1. Agregar useEffect

Reemplaza el archivo completo por:

**Archivo:
src/webparts/projectDashboardXXX/components/ProjectDashboardXXX.tsx**

  -----------------------------------------------------------------------
  import \* as React from 'react';`<br>`{=html}`<br>`{=html}export
  default function ProjectDashboardXXX(): React.ReactElement
  {`<br>`{=html} const \[count, setCount\] =
  React.useState`<number>`{=html}(0);`<br>`{=html}`<br>`{=html}
  React.useEffect(() =\> {`<br>`{=html}
  console.log(`El contador cambió a ${count}`);`<br>`{=html} },
  \[count\]);`<br>`{=html}`<br>`{=html} return (`<br>`{=html}
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

### Paso 2. Comprobar el efecto

Abre las herramientas de desarrollador del navegador con F12. Selecciona
Console. Pulsa `Incrementar` dos veces.

**Resultado esperado.** La consola muestra un mensaje para cada cambio
del estado.

## Actividad 11. Implementar useRef

Utilizar una referencia para colocar el foco en un campo de texto sin
usar el estado para almacenar la referencia.

### Paso 1. Reemplazar el componente

Utiliza este contenido completo:

**Archivo:
src/webparts/projectDashboardXXX/components/ProjectDashboardXXX.tsx**

  --------------------------------------------------------------------------
  import \* as React from 'react';`<br>`{=html}`<br>`{=html}export default
  function ProjectDashboardXXX(): React.ReactElement {`<br>`{=html} const
  \[count, setCount\] = React.useState`<number>`{=html}(0);`<br>`{=html}
  const inputRef =
  React.useRef`<HTMLInputElement>`{=html}(null);`<br>`{=html}`<br>`{=html}
  React.useEffect(() =\> {`<br>`{=html}
  console.log(`El contador cambió a ${count}`);`<br>`{=html} },
  \[count\]);`<br>`{=html}`<br>`{=html} const focusInput = (): void =\>
  {`<br>`{=html} inputRef.current?.focus();`<br>`{=html}
  };`<br>`{=html}`<br>`{=html} return (`<br>`{=html}
  --------------------------------------------------------------------------

  --------------------------------------------------------------------------

**Resultado esperado.** Al seleccionar `Focalizar input`, el cursor se
coloca en el campo de texto.

## Actividad 12. Crear un hook personalizado

Encapsular la lógica de estado para los proyectos en un hook
reutilizable.

### Paso 1. Crear useProjects.ts

En `components/`, crea el archivo.

**Archivo: src/webparts/projectDashboardXXX/components/useProjects.ts**

  -----------------------------------------------------------------------
  import \* as React from 'react';`<br>`{=html}import { IProject } from
  './IProject';`<br>`{=html}`<br>`{=html}export function useProjects():
  {`<br>`{=html} projects: IProject\[\];`<br>`{=html} setProjects:
  React.Dispatch\<React.SetStateAction\<IProject\[\]\>\>;`<br>`{=html}}
  {`<br>`{=html} const \[projects, setProjects\] =
  React.useState\<IProject\[\]\>(\[\]);`<br>`{=html}`<br>`{=html} return
  {`<br>`{=html} projects,`<br>`{=html} setProjects`<br>`{=html}
  };`<br>`{=html}}
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

### Paso 2. Explicar la responsabilidad

El hook mantiene el estado de la colección de proyectos. En una
actividad posterior recibirá una función de carga para obtener datos
desde SharePoint.

**Resultado esperado.** `useProjects.ts` exporta `useProjects` y
devuelve `projects` y `setProjects`.

## Actividad 13. Incorporar Fluent UI

Utilizar componentes de Fluent UI para construir el formulario.

### Paso 1. Reemplazar el componente

Utiliza el siguiente contenido completo.

**Archivo:
src/webparts/projectDashboardXXX/components/ProjectDashboardXXX.tsx**

  --------------------------------------------------------------------------
  import \* as React from 'react';`<br>`{=html}import {`<br>`{=html}
  PrimaryButton,`<br>`{=html} Stack,`<br>`{=html} TextField`<br>`{=html}}
  from '@fluentui/react';`<br>`{=html}`<br>`{=html}export default function
  ProjectDashboardXXX(): React.ReactElement {`<br>`{=html} const \[count,
  setCount\] = React.useState`<number>`{=html}(0);`<br>`{=html} const
  \[projectName, setProjectName\] =
  React.useState`<string>`{=html}('');`<br>`{=html} const inputRef =
  React.useRef`<HTMLInputElement>`{=html}(null);`<br>`{=html}`<br>`{=html}
  React.useEffect(() =\> {`<br>`{=html}
  console.log(`El contador cambió a ${count}`);`<br>`{=html} },
  \[count\]);`<br>`{=html}`<br>`{=html} const focusInput = (): void =\>
  {`<br>`{=html} inputRef.current?.focus();`<br>`{=html}
  };`<br>`{=html}`<br>`{=html} return (`<br>`{=html} \<Stack tokens={{
  childrenGap: 10 }}\>`<br>`{=html}
  --------------------------------------------------------------------------

  --------------------------------------------------------------------------

**Resultado esperado.** El Web Part muestra un `TextField`, dos
`PrimaryButton` y un `Stack`.

## Actividad 14. Crear un input controlado y validar la entrada

Mantener el valor del formulario en el estado de React y rechazar
valores vacíos.

### Paso 1. Actualizar el componente

Utiliza esta versión completa.

**Archivo:
src/webparts/projectDashboardXXX/components/ProjectDashboardXXX.tsx**

  -----------------------------------------------------------------------
  import \* as React from 'react';`<br>`{=html}import {`<br>`{=html}
  MessageBar,`<br>`{=html} MessageBarType,`<br>`{=html}
  PrimaryButton,`<br>`{=html} Stack,`<br>`{=html} TextField`<br>`{=html}}
  from '@fluentui/react';`<br>`{=html}`<br>`{=html}export default
  function ProjectDashboardXXX(): React.ReactElement {`<br>`{=html} const
  \[projectName, setProjectName\] =
  React.useState`<string>`{=html}('');`<br>`{=html} const \[message,
  setMessage\] =
  React.useState`<string>`{=html}('');`<br>`{=html}`<br>`{=html} const
  handleAddProject = (): void =\> {`<br>`{=html} const normalizedName =
  projectName.trim();`<br>`{=html}`<br>`{=html} if (!normalizedName)
  {`<br>`{=html} setMessage('Escribe un nombre de
  proyecto.');`<br>`{=html} return;`<br>`{=html}
  }`<br>`{=html}`<br>`{=html}
  setMessage(`Proyecto preparado: ${normalizedName}`);`<br>`{=html}
  };`<br>`{=html}`<br>`{=html} return (`<br>`{=html} \<Stack tokens={{
  childrenGap: 10 }}\>`<br>`{=html}
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

**Resultado esperado.** Al pulsar `Agregar proyecto` con el campo vacío
aparece el mensaje de validación. Con `Portal SPFx`, aparece
`Proyecto preparado: Portal SPFx`.

## Actividad 15. Conectar SharePoint REST

Obtener los elementos de la lista `Proyectos` mediante el cliente
autenticado de SPFx.

### Paso 1. Preparar la interfaz de propiedades

Reemplaza el contenido de `IProjectDashboardProps.ts` por:

**Archivo:
src/webparts/projectDashboardXXX/components/IProjectDashboardProps.ts**

  -----------------------------------------------------------------------
  import { IProject } from './IProject';`<br>`{=html}`<br>`{=html}export
  interface IProjectDashboardProps {`<br>`{=html} userDisplayName:
  string;`<br>`{=html} getProjects: () =\>
  Promise\<IProject\[\]\>;`<br>`{=html}}
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

### Paso 2. Preparar la función REST

La función se implementará en el Web Part principal porque
`this.context.spHttpClient` pertenece al contexto de SPFx.

**Fragmento:
src/webparts/projectDashboardXXX/ProjectDashboardXXXWebPart.ts**

  -----------------------------------------------------------------------------------------
  private async getProjects(): Promise\<IProject\[\]\> {`<br>`{=html} const url
  =`<br>`{=html} `${this.context.pageContext.web.absoluteUrl}` +`<br>`{=html}
  `/_api/web/lists/getbytitle('Proyectos')/items` +`<br>`{=html}
  `?$select=Id,Title,Owner,Status,Description`;`<br>`{=html}`<br>`{=html} const response =
  await this.context.spHttpClient.get(`<br>`{=html} url,`<br>`{=html}
  SPHttpClient.configurations.v1`<br>`{=html} );`<br>`{=html}`<br>`{=html} if
  (!response.ok) {`<br>`{=html} throw new Error(`<br>`{=html}
  `Error al consultar SharePoint: ${response.status} ${response.statusText}``<br>`{=html}
  );`<br>`{=html} }`<br>`{=html}`<br>`{=html} const data = await
  response.json();`<br>`{=html} return data.value as IProject\[\];`<br>`{=html}}
  -----------------------------------------------------------------------------------------

  -----------------------------------------------------------------------------------------

La URL se construye con la URL absoluta del sitio donde está instalado
el Web Part. No escribas manualmente una URL de tenant dentro del
código.

**Resultado esperado.** La función solicita únicamente `Id`, `Title`,
`Owner`, `Status` y `Description` de la lista `Proyectos`.

## Actividad 16. Pasar los datos de SPFx a React

Separar la obtención de datos del contexto SPFx de la presentación
realizada por React.

### Paso 1. Actualizar el componente React

Reemplaza el archivo completo por:

**Archivo:
src/webparts/projectDashboardXXX/components/ProjectDashboardXXX.tsx**

  -----------------------------------------------------------------------
  import \* as React from 'react';`<br>`{=html}import { PrimaryButton,
  Stack } from '@fluentui/react';`<br>`{=html}import {
  IProjectDashboardProps } from
  './IProjectDashboardProps';`<br>`{=html}`<br>`{=html}export default
  function ProjectDashboardXXX(`<br>`{=html} props:
  IProjectDashboardProps`<br>`{=html}): React.ReactElement {`<br>`{=html}
  const \[projects, setProjects\] =
  React.useState(\[\]);`<br>`{=html}`<br>`{=html} React.useEffect(() =\>
  {`<br>`{=html} void props.getProjects().then(setProjects);`<br>`{=html}
  }, \[props.getProjects\]);`<br>`{=html}`<br>`{=html} return
  (`<br>`{=html} \<Stack tokens={{ childrenGap: 10 }}\>`<br>`{=html}
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

### Paso 2. Pasar las props desde el Web Part

En el método `render()` del Web Part, utiliza `ReactDom.render()` con
`projects`, `userDisplayName` y `onReloadProjects`. La implementación
completa del Web Part se proporcionará después de integrar Graph.

**Resultado esperado.** React recibe datos tipados mediante
`IProjectDashboardProps` y no necesita conocer `SPHttpClient`.

## Actividad 17. Conectar Microsoft Graph

Consultar el perfil del usuario autenticado mediante `Microsoft Graph`.

### Paso 1. Obtener el cliente Graph

En el Web Part principal, utiliza `MSGraphClientV3` proporcionado por
SPFx.

**Fragmento:
src/webparts/projectDashboardXXX/ProjectDashboardXXXWebPart.ts**

  ----------------------------------------------------------------------------------
  const client: MSGraphClientV3 =`<br>`{=html} await
  this.context.msGraphClientFactory.getClient('3');`<br>`{=html}`<br>`{=html}const
  response = await
  client.api('/me').select('displayName').get();`<br>`{=html}`<br>`{=html}const
  userDisplayName = response.displayName as string;
  ----------------------------------------------------------------------------------

  ----------------------------------------------------------------------------------

### Paso 2. Limitar los datos solicitados

La consulta solicita solamente `displayName`, que es el dato utilizado
por la interfaz.

**Resultado esperado.** La llamada a `/me` devuelve el nombre para
mostrar del usuario autenticado.

## Actividad 18. Declarar el permiso User.Read

Solicitar únicamente el permiso de `Microsoft Graph` necesario para
consultar `/me`.

### Paso 1. Abrir package-solution.json

Abre
`config/`package-solution.json\``. Dentro de la propiedad`solution`, agrega`webApiPermissionRequests\`.

**Archivo: config/package-solution.json --- propiedad dentro de
solution**

  -----------------------------------------------------------------------
  "skipFeatureDeployment": false,`<br>`{=html}"webApiPermissionRequests":
  \[`<br>`{=html} {`<br>`{=html} "resource": "Microsoft
  Graph",`<br>`{=html} "scope": "User.Read"`<br>`{=html} }`<br>`{=html}\]
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

No elimines propiedades existentes de `solution`. Agrega la propiedad
con una coma válida respecto de la propiedad anterior.

### Paso 2. Validar el JSON

Guarda el archivo y comprueba que VS Code no muestre un error de
sintaxis JSON.

**Resultado esperado.** La solución declara exactamente
`Microsoft Graph` con el ámbito `User.Read`.

## Actividad 19. Aplicar el ciclo de vida con useEffect

Relacionar montaje y desmontaje del componente funcional con el modelo
de ciclo de vida de componentes de clase.

### Paso 1. Agregar un efecto con limpieza

En \``ProjectDashboardXXX`.tsx\`, utiliza el siguiente patrón:

**Fragmento:
src/webparts/projectDashboardXXX/components/ProjectDashboardXXX.tsx**

  -----------------------------------------------------------------------
  React.useEffect(() =\> {`<br>`{=html} console.log('ProjectDashboardXXX
  montado');`<br>`{=html}`<br>`{=html} return () =\> {`<br>`{=html}
  console.log('ProjectDashboardXXX desmontado');`<br>`{=html}
  };`<br>`{=html}}, \[\]);
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

### Paso 2. Comparar con React clásico

Interpreta la relación de forma conceptual: `componentDidMount()` se
aproxima a un \``useEffect`(..., \[\])\`; un efecto con dependencias
responde a cambios de esas dependencias; la función retornada por el
efecto se utiliza para limpieza.

**Resultado esperado.** La consola registra el montaje y, cuando el
componente deja de estar presente, puede registrar su desmontaje.

## Actividad 20. Convertir useProjects en un hook de carga

Encapsular la carga asíncrona de proyectos en un hook reutilizable.

### Paso 1. Reemplazar useProjects.ts

Utiliza el contenido completo:

**Archivo: src/webparts/projectDashboardXXX/components/useProjects.ts**

  -----------------------------------------------------------------------
  import \* as React from 'react';`<br>`{=html}import { IProject } from
  './IProject';`<br>`{=html}`<br>`{=html}export function
  useProjects(`<br>`{=html} loadProjects: () =\>
  Promise\<IProject\[\]\>`<br>`{=html}): {`<br>`{=html} projects:
  IProject\[\];`<br>`{=html} reload: () =\>
  Promise`<void>`{=html};`<br>`{=html}} {`<br>`{=html} const \[projects,
  setProjects\] =
  React.useState\<IProject\[\]\>(\[\]);`<br>`{=html}`<br>`{=html} const
  reload = React.useCallback(async (): Promise`<void>`{=html} =\>
  {`<br>`{=html} const data = await loadProjects();`<br>`{=html}
  setProjects(data);`<br>`{=html} },
  \[loadProjects\]);`<br>`{=html}`<br>`{=html} React.useEffect(() =\>
  {`<br>`{=html} void reload();`<br>`{=html} },
  \[reload\]);`<br>`{=html}`<br>`{=html} return {`<br>`{=html}
  projects,`<br>`{=html} reload`<br>`{=html} };`<br>`{=html}}
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

### Paso 2. Hacer estable la función de carga

En la implementación final, `getProjects` se define como una función
flecha de instancia en el Web Part, por lo que su referencia es estable.
El hook la recibe como dependencia para controlar cuándo debe recargar
los datos.

Patrón conceptual

  -----------------------------------------------------------------------
  const loadProjects = React.useCallback(`<br>`{=html} () =\>
  this.props.getProjects(),`<br>`{=html} \[this.props\]`<br>`{=html});
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

Este fragmento es conceptual; la implementación final utiliza la función
estable proporcionada por el Web Part.

**Resultado esperado.** El hook recibe una función de carga y administra
el estado de los proyectos.

## Actividad 21. Construir el formulario completo

Integrar estado, validación, Fluent UI, proyectos obtenidos desde
SharePoint y usuario autenticado.

### Paso 1. Preparar el componente final

Reemplaza \``ProjectDashboardXXX`.tsx\` por el siguiente archivo
completo.

**Archivo:
src/webparts/projectDashboardXXX/components/ProjectDashboardXXX.tsx**

  ----------------------------------------------------------------------------------------
  import \* as React from 'react';`<br>`{=html}import {`<br>`{=html}
  MessageBar,`<br>`{=html} MessageBarType,`<br>`{=html} PrimaryButton,`<br>`{=html}
  Stack,`<br>`{=html} TextField`<br>`{=html}} from '@fluentui/react';`<br>`{=html}import {
  IProject } from './IProject';`<br>`{=html}import { IProjectDashboardProps } from
  './IProjectDashboardProps';`<br>`{=html}import { useProjects } from
  './useProjects';`<br>`{=html}`<br>`{=html}export default function
  ProjectDashboardXXX(`<br>`{=html} props: IProjectDashboardProps`<br>`{=html}):
  React.ReactElement {`<br>`{=html} const \[count, setCount\] =
  React.useState`<number>`{=html}(0);`<br>`{=html} const \[projectName, setProjectName\] =
  React.useState`<string>`{=html}('');`<br>`{=html} const \[owner, setOwner\] =
  React.useState`<string>`{=html}('');`<br>`{=html} const \[message, setMessage\] =
  React.useState`<string>`{=html}('');`<br>`{=html}`<br>`{=html} const inputRef =
  React.useRef`<HTMLInputElement>`{=html}(null);`<br>`{=html} const { projects, reload } =
  useProjects(props.getProjects);`<br>`{=html}`<br>`{=html} const handleAddProject = ():
  void =\> {`<br>`{=html} const normalizedName = projectName.trim();`<br>`{=html} const
  normalizedOwner = owner.trim();`<br>`{=html}`<br>`{=html} if (!normalizedName)
  {`<br>`{=html} setMessage('Escribe un nombre de proyecto.');`<br>`{=html}
  return;`<br>`{=html} }`<br>`{=html}`<br>`{=html} if (!normalizedOwner) {`<br>`{=html}
  setMessage('Escribe un responsable.');`<br>`{=html} return;`<br>`{=html}
  }`<br>`{=html}`<br>`{=html} setMessage(`<br>`{=html}
  `Proyecto preparado: ${normalizedName} — Responsable: ${normalizedOwner}``<br>`{=html}
  );`<br>`{=html} };`<br>`{=html}`<br>`{=html} const focusInput = (): void =\>
  {`<br>`{=html} inputRef.current?.focus();`<br>`{=html} };`<br>`{=html}`<br>`{=html}
  return (`<br>`{=html} \<Stack tokens={{ childrenGap: 10 }}\>`<br>`{=html}
  ----------------------------------------------------------------------------------------

  ----------------------------------------------------------------------------------------

## Actividad 22. Aplicar controles básicos de seguridad

Aplicar mínimo privilegio, evitar almacenamiento manual de tokens,
validar entradas y utilizar HTTPS.

### Paso 1. Mantener el mínimo privilegio

El único permiso de `Microsoft Graph` solicitado por este laboratorio es
`User.Read`, porque la aplicación únicamente consulta `/me` para obtener
`displayName`.

### Paso 2. No almacenar tokens manualmente

No agregues código que guarde tokens de acceso en `localStorage`,
`sessionStorage`, cookies creadas por el Web Part o archivos del
proyecto. SPFx administra el acceso a los servicios autenticados
mediante sus clientes y contexto.

### Paso 3. Validar la entrada

Antes de aceptar el nombre y el responsable, elimina espacios al inicio
y al final y rechaza cadenas vacías. No insertes valores proporcionados
por el usuario mediante `innerHTML`.

### Paso 4. Utilizar HTTPS

Las llamadas de desarrollo y las llamadas a Microsoft 365 deben utilizar
los mecanismos HTTPS proporcionados por SPFx. No cambies `https` por
`http` en `serve.json`.

**Resultado esperado.** El proyecto solicita únicamente el permiso
requerido y el formulario rechaza entradas vacías sin almacenar tokens
manualmente.

## Actividad 23. Revisar optimizaciones básicas de rendimiento

Revisar patrones para evitar llamadas innecesarias, cargar datos
independientes en paralelo y comprender el propósito de lazy loading.

### Paso 1. Cargar datos independientes en paralelo

Analiza el patrón `Promise.all` del siguiente ejemplo. En este
laboratorio se estudia como patrón de optimización; no lo incorpores al
Web Part final porque la carga de usuario y proyectos se realiza
mediante mecanismos distintos.

**Fragmento:
src/webparts/projectDashboardXXX/ProjectDashboardXXXWebPart.ts**

  -----------------------------------------------------------------------
  const \[projects, userDisplayName\] = await Promise.all(\[`<br>`{=html}
  this.getProjects(),`<br>`{=html}
  this.getUserDisplayName()`<br>`{=html}\]);
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

### Paso 2. Evitar llamadas en cada renderizado

El hook `useProjects` recibe una función estable mediante
`React.useCallback`. Su efecto depende de esa función y no se ejecuta
por cada cambio de estado del formulario.

### Paso 3. Comprender lazy loading

El laboratorio solo requiere comprender el patrón. No agregues
`React.lazy` al Web Part final porque no existe un segundo componente
que necesitemos cargar de forma diferida.

Ejemplo conceptual; no crear este archivo en el laboratorio

  -----------------------------------------------------------------------
  const DetalleProyecto = React.lazy(`<br>`{=html} () =\>
  import('./DetalleProyecto')`<br>`{=html});
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

**Resultado esperado.** Identificas cuándo `Promise.all` puede reducir
el tiempo total de espera y verificas que el Web Part final no repite la
consulta de proyectos por cada cambio del formulario.

## Actividad 24. Revisar TypeScript estricto

Comprobar que la configuración del proyecto conserva las reglas de
TypeScript proporcionadas por el scaffolding.

### Paso 1. Abrir tsconfig.json

En la raíz del proyecto abre `tsconfig.json`. Busca la propiedad
`compilerOptions` y revisa la configuración generada por SPFx.

### Paso 2. No modificar la configuración sin necesidad

No agregues ni elimines `strict` únicamente para ocultar errores del
laboratorio. Si el archivo generado no contiene `"strict": true`,
conserva la configuración generada por la versión de SPFx utilizada.

**Resultado esperado.** La configuración de TypeScript permanece
compatible con el proyecto generado y los errores de tipos se detectan
durante la compilación.

## Actividad 25. Compilar la solución

Comprobar que TypeScript, React, Fluent UI, SPFx y los archivos de
configuración pueden procesarse conjuntamente.

### Paso 1. Guardar todos los archivos

En VS Code selecciona Archivo \> Guardar todo. Corrige primero cualquier
indicador rojo de error de TypeScript.

### Paso 2. Compilar con Heft

Abre una terminal en la raíz del proyecto y ejecuta:

  heft build
  ------------

**Resultado esperado.** La ejecución termina sin errores. Si aparece un
error, corrígelo antes de continuar con el empaquetado.

## Actividad 26. Generar el paquete .sppkg

Crear el artefacto de producción que se publicará en el App Catalog.

### Paso 1. Generar el paquete

Desde la raíz del proyecto ejecuta:

  heft package-solution --production
  ------------------------------------

### Paso 2. Localizar el paquete

En VS Code, expande `sharepoint/solution/`. Localiza el archivo `.sppkg`
generado. El nombre se deriva de la configuración de la solución; no
asumas que será exactamente `projectdashboard.sppkg`.

**Resultado esperado.** Existe un único paquete `.sppkg` correspondiente
a la ejecución que acabas de realizar.

## Actividad 27. Publicar la solución y aprobar User.Read

Entregar el paquete al instructor para su publicación en el App Catalog
compartido y completar la aprobación administrativa del permiso
solicitado.

### Paso 1. Entregar el paquete al instructor

Entrega al instructor el archivo `.sppkg` generado en
`sharepoint/solution/`. El participante no administra el App Catalog
compartido.

### Paso 2. Publicación en el App Catalog

El instructor carga el archivo `.sppkg` en la biblioteca
`Apps for SharePoint` del App Catalog compartido.

### Paso 3. Confirmar la implementación

El instructor confirma la implementación de la solución cuando
SharePoint muestre la ventana correspondiente.

### Paso 4. Revisar Acceso de API

El instructor abre SharePoint Admin Center \> Más características \>
Aplicaciones \> Acceso de API y localiza la solicitud pendiente para
`Microsoft Graph` con el permiso `User.Read`.

### Paso 5. Aprobar el permiso

El instructor selecciona la solicitud `User.Read`, elige Aprobar y
confirma la aprobación.

**Resultado esperado.** El paquete aparece en `Apps for SharePoint` y la
solicitud de `Microsoft Graph / User.Read` aparece como aprobada.

## Actividad 28. Agregar ProjectDashboardXXX a la página

Agregar el Web Part publicado a la página moderna del sitio de
laboratorio.

### Paso 1. Abrir la página

Abre `` Portal-`Proyectos`XXX `` y entra en la página
`Panel de proyectos` creada en la Actividad 1.

### Paso 2. Editar la página

Selecciona Editar.

### Paso 3. Insertar el Web Part

Selecciona el botón `+` de una sección de la página. En el selector de
Web Parts, busca `ProjectDashboardXXX` y selecciónalo.

### Paso 4. Publicar la página

Selecciona Publicar o Volver a publicar, según el estado de la página.

**Resultado esperado.** La página publicada contiene el Web Part
`ProjectDashboardXXX`.

## Actividad 29. Validar el funcionamiento completo

Comprobar la interfaz, el estado React, el input controlado, SharePoint
REST, `Microsoft Graph` y Fluent UI.

### Paso 1. Validar el usuario

En el Web Part verifica que aparezca `Usuario:` seguido del nombre de la
cuenta con la que iniciaste sesión.

### Paso 2. Validar los proyectos

Verifica que aparezcan los tres registros creados en la lista
`Proyectos`: `Portal SPFx`, `Dashboard Viva` y `Centro Documental`.

### Paso 3. Validar el input controlado

Escribe `Nuevo proyecto` en `Nombre del proyecto`. El texto escrito debe
permanecer sincronizado con el estado del componente.

### Paso 4. Validar la validación

Borra el contenido de `Nombre del proyecto` y pulsa `Agregar proyecto`.
Debe aparecer el mensaje `Escribe un nombre de proyecto.`. Escribe
`Nuevo proyecto` y `Responsable` en el segundo campo; después pulsa
`Agregar proyecto`.

### Paso 5. Validar Fluent UI

Comprueba visualmente que los campos y botones utilizados por el
formulario corresponden a componentes de Fluent UI y que la interfaz se
renderiza sin errores.

### Paso 6. Validar Graph

Abre F12 \> Console y confirma que no existen errores de autorización
relacionados con `/me`. El nombre mostrado en la interfaz debe
corresponder al usuario autenticado.

### Paso 7. Validar REST

Comprueba que la lista de proyectos corresponde a los registros
existentes en `Proyectos`. Si agregas un cuarto elemento directamente en
la lista de SharePoint, utiliza la función de recarga para comprobar que
la nueva consulta lo devuelve.

**Resultado esperado.** El Web Part funciona en una página moderna de
SharePoint y demuestra la integración de React, Fluent UI, SharePoint
REST y Microsoft Graph.

## Código completo de los archivos principales

Los siguientes bloques representan el estado final de los archivos
principales utilizados en el laboratorio. Si el generador creó
propiedades adicionales en los archivos, consérvalas salvo cuando este
laboratorio indique explícitamente reemplazar el archivo completo.

**Archivo completo:
src/webparts/projectDashboardXXX/components/IProject.ts**

  -----------------------------------------------------------------------
  export interface IProject {`<br>`{=html} Id: number;`<br>`{=html}
  Title: string;`<br>`{=html} Owner: string;`<br>`{=html} Status:
  string;`<br>`{=html} Description: string;`<br>`{=html}}
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

**Archivo completo:
src/webparts/projectDashboardXXX/components/IProjectDashboardProps.ts**

  -----------------------------------------------------------------------
  import { IProject } from './IProject';`<br>`{=html}`<br>`{=html}export
  interface IProjectDashboardProps {`<br>`{=html} userDisplayName:
  string;`<br>`{=html} getProjects: () =\>
  Promise\<IProject\[\]\>;`<br>`{=html}}
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

**Archivo completo:
src/webparts/projectDashboardXXX/components/useProjects.ts**

  -----------------------------------------------------------------------
  import \* as React from 'react';`<br>`{=html}import { IProject } from
  './IProject';`<br>`{=html}`<br>`{=html}export function
  useProjects(`<br>`{=html} loadProjects: () =\>
  Promise\<IProject\[\]\>`<br>`{=html}): {`<br>`{=html} projects:
  IProject\[\];`<br>`{=html} reload: () =\>
  Promise`<void>`{=html};`<br>`{=html}} {`<br>`{=html} const \[projects,
  setProjects\] =
  React.useState\<IProject\[\]\>(\[\]);`<br>`{=html}`<br>`{=html} const
  reload = React.useCallback(async (): Promise`<void>`{=html} =\>
  {`<br>`{=html} const data = await loadProjects();`<br>`{=html}
  setProjects(data);`<br>`{=html} },
  \[loadProjects\]);`<br>`{=html}`<br>`{=html} React.useEffect(() =\>
  {`<br>`{=html} void reload();`<br>`{=html} },
  \[reload\]);`<br>`{=html}`<br>`{=html} return {`<br>`{=html}
  projects,`<br>`{=html} reload`<br>`{=html} };`<br>`{=html}}
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

**Archivo completo:
src/webparts/projectDashboardXXX/components/ProjectDashboardXXX.tsx**

  ----------------------------------------------------------------------------------------
  import \* as React from 'react';`<br>`{=html}import {`<br>`{=html}
  MessageBar,`<br>`{=html} MessageBarType,`<br>`{=html} PrimaryButton,`<br>`{=html}
  Stack,`<br>`{=html} TextField`<br>`{=html}} from '@fluentui/react';`<br>`{=html}import {
  IProject } from './IProject';`<br>`{=html}import { IProjectDashboardProps } from
  './IProjectDashboardProps';`<br>`{=html}import { useProjects } from
  './useProjects';`<br>`{=html}`<br>`{=html}export default function
  ProjectDashboardXXX(`<br>`{=html} props: IProjectDashboardProps`<br>`{=html}):
  React.ReactElement {`<br>`{=html} const \[count, setCount\] =
  React.useState`<number>`{=html}(0);`<br>`{=html} const \[projectName, setProjectName\] =
  React.useState`<string>`{=html}('');`<br>`{=html} const \[owner, setOwner\] =
  React.useState`<string>`{=html}('');`<br>`{=html} const \[message, setMessage\] =
  React.useState`<string>`{=html}('');`<br>`{=html}`<br>`{=html} const inputRef =
  React.useRef`<HTMLInputElement>`{=html}(null);`<br>`{=html} const { projects, reload } =
  useProjects(props.getProjects);`<br>`{=html}`<br>`{=html} const handleAddProject = ():
  void =\> {`<br>`{=html} const normalizedName = projectName.trim();`<br>`{=html} const
  normalizedOwner = owner.trim();`<br>`{=html}`<br>`{=html} if (!normalizedName)
  {`<br>`{=html} setMessage('Escribe un nombre de proyecto.');`<br>`{=html}
  return;`<br>`{=html} }`<br>`{=html}`<br>`{=html} if (!normalizedOwner) {`<br>`{=html}
  setMessage('Escribe un responsable.');`<br>`{=html} return;`<br>`{=html}
  }`<br>`{=html}`<br>`{=html} setMessage(`<br>`{=html}
  `Proyecto preparado: ${normalizedName} — Responsable: ${normalizedOwner}``<br>`{=html}
  );`<br>`{=html} };`<br>`{=html}`<br>`{=html} const focusInput = (): void =\>
  {`<br>`{=html} inputRef.current?.focus();`<br>`{=html} };`<br>`{=html}`<br>`{=html}
  return (`<br>`{=html} \<Stack tokens={{ childrenGap: 10 }}\>`<br>`{=html}
  ----------------------------------------------------------------------------------------

  ----------------------------------------------------------------------------------------

**Archivo completo:
src/webparts/projectDashboardXXX/ProjectDashboardXXXWebPart.ts**

  -----------------------------------------------------------------------------------------
  import \* as React from 'react';`<br>`{=html}import \* as ReactDom from
  'react-dom';`<br>`{=html}import { Version } from
  '@microsoft/sp-core-library';`<br>`{=html}import { BaseClientSideWebPart } from
  '@microsoft/sp-webpart-base';`<br>`{=html}import {`<br>`{=html}
  MSGraphClientV3,`<br>`{=html} SPHttpClient`<br>`{=html}} from
  '@microsoft/sp-http';`<br>`{=html}`<br>`{=html}import ProjectDashboardXXX from
  './components/ProjectDashboardXXX';`<br>`{=html}import { IProject } from
  './components/IProject';`<br>`{=html}import { IProjectDashboardProps } from
  './components/IProjectDashboardProps';`<br>`{=html}`<br>`{=html}export interface
  IProjectDashboardXXXWebPartProps {}`<br>`{=html}`<br>`{=html}export default class
  ProjectDashboardXXXWebPart`<br>`{=html} extends
  BaseClientSideWebPart`<IProjectDashboardXXXWebPartProps>`{=html}
  {`<br>`{=html}`<br>`{=html} private readonly getProjects = async ():
  Promise\<IProject\[\]\> =\> {`<br>`{=html} const url =`<br>`{=html}
  `${this.context.pageContext.web.absoluteUrl}` +`<br>`{=html}
  `/_api/web/lists/getbytitle('Proyectos')/items` +`<br>`{=html}
  `?$select=Id,Title,Owner,Status,Description`;`<br>`{=html}`<br>`{=html} const response =
  await this.context.spHttpClient.get(`<br>`{=html} url,`<br>`{=html}
  SPHttpClient.configurations.v1`<br>`{=html} );`<br>`{=html}`<br>`{=html} if
  (!response.ok) {`<br>`{=html} throw new Error(`<br>`{=html}
  `Error al consultar SharePoint: ${response.status} ${response.statusText}``<br>`{=html}
  );`<br>`{=html} }`<br>`{=html}`<br>`{=html} const data = await
  response.json();`<br>`{=html} return data.value as IProject\[\];`<br>`{=html}
  };`<br>`{=html}`<br>`{=html} private readonly getUserDisplayName = async ():
  Promise`<string>`{=html} =\> {`<br>`{=html} const client: MSGraphClientV3 =`<br>`{=html}
  await this.context.msGraphClientFactory.getClient('3');`<br>`{=html}`<br>`{=html} const
  response = await client`<br>`{=html} .api('/me')`<br>`{=html}
  .select('displayName')`<br>`{=html} .get();`<br>`{=html}`<br>`{=html} return
  response.displayName as string;`<br>`{=html} };`<br>`{=html}`<br>`{=html} private
  userDisplayName: string = '';`<br>`{=html}`<br>`{=html} protected async onInit():
  Promise`<void>`{=html} {`<br>`{=html} await super.onInit();`<br>`{=html}
  this.userDisplayName = await this.getUserDisplayName();`<br>`{=html}
  }`<br>`{=html}`<br>`{=html} public render(): void {`<br>`{=html} const element:
  React.ReactElement`<IProjectDashboardProps>`{=html} =`<br>`{=html}
  React.createElement(ProjectDashboardXXX, {`<br>`{=html} userDisplayName:
  this.userDisplayName,`<br>`{=html} getProjects: this.getProjects`<br>`{=html}
  });`<br>`{=html}`<br>`{=html} ReactDom.render(element, this.domElement);`<br>`{=html}
  }`<br>`{=html}`<br>`{=html} protected onDispose(): void {`<br>`{=html}
  ReactDom.unmountComponentAtNode(this.domElement);`<br>`{=html}
  }`<br>`{=html}`<br>`{=html} protected get dataVersion(): Version {`<br>`{=html} return
  Version.parse('1.0');`<br>`{=html} }`<br>`{=html}}
  -----------------------------------------------------------------------------------------

  -----------------------------------------------------------------------------------------

**Fragmento final: config/package-solution.json --- dentro de solution**

  -----------------------------------------------------------------------
  "skipFeatureDeployment": false,`<br>`{=html}"webApiPermissionRequests":
  \[`<br>`{=html} {`<br>`{=html} "resource": "Microsoft
  Graph",`<br>`{=html} "scope": "User.Read"`<br>`{=html} }`<br>`{=html}\]
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

## Comprobación final

Marca cada criterio únicamente cuando hayas comprobado físicamente el
resultado en el entorno del laboratorio.

☐ App Catalog disponible.

☐ Sitio `` Portal-`Proyectos`XXX `` creado y accesible.

☐ Página `Panel de proyectos` creada y publicada.

☐ Lista `Proyectos` creada con las columnas requeridas.

☐ Lista `Proyectos` contiene los tres registros de prueba.

☐ Node.js 22.23.2 verificado.

☐ Proyecto SPFx 1.23.2 creado mediante Yeoman.

☐ React 17.0.1 disponible.

☐ Fluent UI disponible.

☐ Certificado de desarrollo confiado mediante Heft.

☐ `ProjectDashboardXXX` ejecutado en el entorno de prueba.

☐ `useState` implementado y probado.

☐ `useEffect` implementado y probado.

☐ `useRef` implementado y probado.

☐ Hook personalizado `useProjects` implementado.

☐ Fluent UI implementado.

☐ Input controlado implementado.

☐ Validación de entradas implementada.

☐ SharePoint REST devuelve los elementos de `Proyectos`.

☐ `Microsoft Graph` `/me` devuelve el usuario autenticado.

☐ Solicitud `` Microsoft Graph` / `User.Read `` aprobada.

☐ Patrón `Promise.all` revisado como técnica de optimización.

☐ Solución compilada con `heft build`.

☐ Paquete `.sppkg` generado.

☐ Paquete publicado en `Apps for SharePoint`.

☐ `ProjectDashboardXXX` agregado a la página `Panel de proyectos`.

☐ Usuario autenticado visible.

☐ `Proyectos` visibles desde SharePoint.

☐ Formulario y validación funcionando.


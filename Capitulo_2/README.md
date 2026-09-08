Lab 2 — Desarrollo de soluciones SPFx con React y Heft

**Duración sugerida: 90 minutos**

**Objetivo**

Preparar un ambiente de desarrollo reproducible para SharePoint
Framework (SPFx), crear un Web Part con React y TypeScript, comprender
la estructura del proyecto, compilar y ejecutar la solución mediante
Heft, registrar cambios con Git, generar el paquete .sppkg y completar
el proceso de publicación controlada en SharePoint Online.

**Alcance**

El módulo recorre el ciclo práctico desde la preparación del equipo
hasta la validación de un Web Part publicado. Se utiliza SPFx 1.23.2 con
el toolchain moderno basado en Heft. El proyecto se crea para SharePoint
Online y utiliza React 17.0.1. El módulo incluye una introducción breve
al cambio histórico de Gulp a Heft, pero no crea ni migra un proyecto
Gulp.

**Nomenclatura del participante**

XXX representa las iniciales del participante. Utiliza el sufijo XXX en
los recursos que pueden entrar en conflicto dentro del tenant
compartido.

- Solución: spfx-lab2-webpart-XXX

- Web Part: HelloSpfxXXX

- Sitio de validación: Portal-ProyectosXXX, cuando el instructor asigne
  un sitio específico.

- Lista de referencia, si se utiliza en actividades posteriores:
  Proyectos.

**Requisitos**

- Windows con acceso para instalar Node.js y herramientas de desarrollo.

- Node.js 22.23.2 y npm disponibles en PATH.

- .4 o posterior para PnP.PowerShell.

- Git y Visual Studio Code instalados.

- Conexión a Internet para instalar paquetes npm.

- Cuenta de Microsoft 365 con acceso al sitio de SharePoint Online
  asignado.

- El App Catalog del tenant es administrado por el instructor; el
  participante no crea ni administra el App Catalog.

**Baseline técnico del módulo**

| Componente            | Versión / valor                                            | Uso                                       |
|-----------------------|------------------------------------------------------------|-------------------------------------------|
| Node.js               | 22.23.2                                                    | Runtime del toolchain                     |
| npm                   | 10.9.8                                                     | Gestión de paquetes                       |
| SPFx                  | 1.23.2                                                     | Framework                                 |
| React                 | 17.0.1                                                     | UI del Web Part                           |
| TypeScript            | 5.8.x                                                      | Tipado y compilación                      |
| Heft                  | global disponible; proyecto resuelve su versión compatible | Build y desarrollo                        |
| PowerShell            | 7.4+                                                       | PnP.PowerShell                            |
| PnP.PowerShell        | 3.x                                                        | Interacción administrativa con SharePoint |
| CLI for Microsoft 365 | 11.x                                                       | Comandos Microsoft 365                    |
| Git                   | 2.x                                                        | Control de versiones                      |

La matriz oficial de compatibilidad de Microsoft indica para SPFx 1.23.2
Node.js v22, TypeScript 2.9–5.8 y React 17.0.1. El proyecto generado
debe conservar las versiones que el scaffolding instala; no actualices
React, TypeScript u otras dependencias del proyecto por cuenta propia
durante el laboratorio.

ACTIVIDAD 1 — Ajustar Node.js al baseline del laboratorio

**Objetivo.** Dejar el equipo en Node.js 22.23.2 antes de instalar o
reinstalar las herramientas globales.

**Comprobar la versión actual.**

node --version

npm --version

where.exe node

where.exe npm

**Si Node.js ya muestra v22.23.2, conserva la instalación y continúa con
la verificación.**

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image1.png"
style="width:2.54804in;height:1.5704in" />

**Instalar las herramientas globales después de cambiar de versión de
Node.js.**

npm install -g yo @microsoft/generator-sharepoint@1.23.2 @rushstack/heft
@pnp/cli-microsoft365

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image2.png"
style="width:6.03454in;height:1.91009in" />

ACTIVIDAD 2 — Verificar herramientas

**Objetivo.** Comprobar que las herramientas que se utilizarán durante
el módulo responden desde la sesión de .

git --version

code --version

pwsh --version

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image3.png"
style="width:3.99014in;height:1.62523in" />

**Verificar las herramientas globales.**

npm list -g @microsoft/generator-sharepoint --depth=0

npm list -g @rushstack/heft --depth=0

m365 version

**Verificar PnP.PowerShell.**

Get-InstalledModule PnP.PowerShell

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image4.png"
style="width:6.9in;height:1.90833in" />

**Si PnP.PowerShell no está instalado.**

Install-Module PnP.PowerShell -Scope CurrentUser

Get-InstalledModule PnP.PowerShell

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image5.png"
style="width:6.9in;height:0.62222in" />

ACTIVIDAD 3 — Preparar el generador SPFx 1.23.2

**Objetivo.** Comprobar que Yeoman y el generador fijado para el módulo
están disponibles antes de crear la solución.

npm install -g yo @microsoft/generator-sharepoint@1.23.2

npm list -g @microsoft/generator-sharepoint --depth=0

yo --version

yo @microsoft/sharepoint --help

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image6.png"
style="width:6.9in;height:1.33958in" />

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image7.png"
style="width:6.9in;height:2.78958in" />

No uses una versión diferente del generador para crear el proyecto del
laboratorio. La versión del generador determina el scaffolding y el
conjunto de dependencias que se instalarán.

ACTIVIDAD 4 — Crear el proyecto SPFx

**Objetivo.** Generar la solución spfx-lab2-webpart-XXX con un Web Part
React para SharePoint Online.

**Crear la carpeta de trabajo.**

New-Item -ItemType Directory -Path C:\SPFx -Force

Set-Location C:\SPFx

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image8.png"
style="width:6.6676in;height:2.61495in" />

**Ejecutar el generador.**

yo @microsoft/sharepoint

En el asistente interactivo, utiliza los valores siguientes cuando
aparezcan:

- Solution name: spfx-lab2-webpart-XXX

- Client-side component: WebPart

- Web Part name: HelloSpfxXXX

- Template: React

  <img src="/mnt/data/Laboratorio2_Markdown/media/media/image9.png"
  style="width:6.9in;height:3.43681in" />

  <img src="/mnt/data/Laboratorio2_Markdown/media/media/image10.png"
  style="width:6.9in;height:2.87986in" />

Durante la creación de un proyecto, el generador de SharePoint solicita
seleccionar el tipo de componente que se desea desarrollar. Las opciones
principales son:

| Componente              | Descripción                                                                                                                                                                                                                                                        |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WebPart                 | Componente visual que se agrega a una página de SharePoint. Permite construir interfaces interactivas con React, HTML, CSS y TypeScript, y puede consumir datos de SharePoint, Microsoft Graph u otros servicios. Es el componente utilizado en este laboratorio.  |
| Extension               | Componente que permite extender o modificar determinadas experiencias de SharePoint sin crear un Web Part tradicional. Puede utilizarse, por ejemplo, para agregar acciones a listas, personalizar campos o ejecutar código en determinados puntos de la interfaz. |
| Library                 | Componente destinado a crear una biblioteca de código reutilizable para otros componentes SPFx. Es apropiado cuando se desea compartir funciones, clases, componentes o lógica común entre diferentes soluciones.                                                  |
| Adaptive Card Extension | Componente orientado a Viva Connections que utiliza Adaptive Cards para presentar información y acciones de forma compacta e interactiva. Puede utilizarse para construir experiencias que se integren en el dashboard de Viva Connections.                        |

En SPFx 1.23.2 el asistente interactivo no presenta la antigua pregunta
de Tenant-wide deployment. La configuración de ámbito se comprobará y
ajustará explícitamente en la configuración de empaquetado en una
actividad posterior.

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image11.png"
style="width:4.69749in;height:3.5217in" />

ACTIVIDAD 5 — Configurar el proyecto para un tenant compartido

**Objetivo.** Evitar que la solución del participante se configure para
despliegue tenant-wide en el App Catalog compartido. La intención es
evitar conflictos cuando varios participantes trabajan dentro del mismo
tenant, especialmente con nombres de soluciones, componentes y paquetes
que se agregan al App Catalog compartido.

Como buena práctica, en un tenant compartido, los nombres de las
soluciones y componentes deben permitir identificar inequívocamente su
propietario o propósito.

**Abrir la configuración de empaquetado.**

Ruta del archivo

C:\SPFx\spfx-lab2-webpart-XXX\config\package-solution.json

Abre el archivo package-solution.json y localiza la propiedad
skipFeatureDeployment dentro de solution.

Fragmento que debe quedar en package-solution.json

"skipFeatureDeployment": false

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image12.png"
style="width:4.5162in;height:3.14989in" />

Si la propiedad aparece con true, cámbiala a false. Si el generador no
la muestra, agrega la propiedad dentro del objeto solution, sin eliminar
otras propiedades generadas por el scaffolding.

**Comprobar la configuración.**

Set-Location C:\SPFx\spfx-lab2-webpart-XXX

Get-Content .\config\package-solution.json

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image13.png"
style="width:6.9in;height:2.35972in" />

La solución podrá publicarse en el App Catalog sin solicitar su
activación automática en todos los sitios.

ACTIVIDAD 6 — Reconocer la estructura del proyecto

**Objetivo.** Identificar las carpetas y archivos principales creados
por el scaffolding moderno basado en Heft.

**Abrir el proyecto en Visual Studio Code.**

Set-Location C:\SPFx\spfx-lab2-webpart-XXX

code .

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image14.png"
style="width:6.9in;height:2.51806in" />

**Revisar las carpetas principales.**

- src\webparts\helloSpfxXXX\\

- config\\

- node_modules\\

**Revisar archivos.**

- package.json

- tsconfig.json

- config\rig.json

- config\package-solution.json

- config\serve.json

**Identificar el componente.**

Ruta

src\webparts\helloSpfxXXX\components\HelloSpfxXXX.tsx

src\webparts\helloSpfxXXX\components\IHelloSpfxXXXProps.ts

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image15.png"
style="width:6.89583in;height:4.89583in" />

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image16.png"
style="width:6.9in;height:2.18403in" />

El proyecto moderno utiliza config/rig.json para referenciar el rig de
SPFx. No crees un config/heft.json para este laboratorio. Un heft.json
de proyecto solo es necesario cuando se desea extender explícitamente la
configuración compartida del rig.

**Resultado esperado.** Puedes distinguir código fuente, configuración,
dependencias, configuración de TypeScript y archivos del toolchain.

ACTIVIDAD 7 — Trabajar con TypeScript y tipado estático

**Objetivo.** Definir una interfaz, crear un objeto tipado y observar
cómo TypeScript detecta una incompatibilidad.

**Abrir el archivo.**

Archivo

src\webparts\helloSpfxXXX\components\HelloSpfxXXX.tsx

**Incorporar la interfaz y el objeto.**

Agregar antes del componente React

interface IProject {

id: number;

name: string;

owner: string;

}

const project: IProject = {

id: 1,

name: "Portal SPFx",

owner: "Laboratorio"

};

console.log(project.name);

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image17.png"
style="width:6.89583in;height:5.20833in" />

Guarda el archivo. Desde la terminal integrada de Visual Studio Code,
asegúrate de encontrarte en la raíz del proyecto:

> Set-Location C:\SPFx\spfx-lab2-webpart-XXX
>
> heft build

El proyecto debe compilar correctamente.

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image18.png"
style="width:6.9in;height:4.76944in" />

**Provocar el error de tipos.**

Cambia id: 1 por id: "1" y guarda el archivo.

Desde la raíz del proyecto

Set-Location C:\SPFx\spfx-lab2-webpart-XXX

heft build

Observa el error de TypeScript. Después restaura id: 1.

El build identifica que string no es compatible con number y, después de
restaurar el valor, el proyecto queda listo para continuar.

Regresa el valor de id a 1. Sin comillas.

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image19.png"
style="width:6.9in;height:2.08611in" />

ACTIVIDAD 8 — Implementar estado con React.useState

**Objetivo.** Agregar estado local al Web Part y actualizar la interfaz
mediante un botón.

**Declarar el estado dentro del componente.**

Archivo: src\webparts\helloSpfxXXX\components\HelloSpfxXXX.tsx

const \[count, setCount\] = React.useState(0);

**Mostrar el estado.**

Dentro del JSX

\<p\>Has hecho clic {count} veces.\</p\>

**Agregar el botón.**

Dentro del JSX

\<button onClick={() =\> setCount(count + 1)}\>

Incrementar

\</button\>

**Código completo del componente.**

Archivo: src\webparts\helloSpfxXXX\components\HelloSpfxXXX.tsx

import \* as React from "react";

import { IHelloSpfxXXXProps } from "./IHelloSpfxXXXProps";

interface IProject {

id: number;

name: string;

owner: string;

}

const project: IProject = {

id: 1,

name: "Portal SPFx",

owner: "Laboratorio"

};

const HelloSpfxXXX: React.FC\<IHelloSpfxXXXProps\> = (props) =\> {

const \[count, setCount\] = React.useState(0);

console.log(project.name);

return (

\<div\>

\<h2\>¡Hola SPFx con React!\</h2\>

\<p\>Proyecto: {project.name}\</p\>

\<p\>Propietario: {project.owner}\</p\>

\<p\>Usuario: {props.userDisplayName ?? "Participante"}\</p\>

\<p\>Has hecho clic {count} veces.\</p\>

\<button onClick={() =\> setCount(count + 1)}\>

Incrementar

\</button\>

\</div\>

);

};

export default HelloSpfxXXX;

Guarda los cambios.

**Compilar el proyecto**

Abre la terminal integrada de Visual Studio Code y sitúate en la raíz
del proyecto:

Set-Location C:\SPFx\spfx-lab2-webpart-XXX

heft build

Confirma que la compilación finaliza correctamente.

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image20.png"
style="width:6.9in;height:1.66875in" />

ACTIVIDAD 9 — Instalar o sincronizar dependencias y compilar con Heft

**Objetivo.** Comprobar que el proyecto procesa correctamente
TypeScript, React, ESLint y Webpack.

Set-Location C:\SPFx\spfx-lab2-webpart-XXX

npm install

heft build

**Resultado esperado.** El build termina sin errores. La salida de Heft
muestra las tareas de compilación y el uso de TypeScript, ESLint y
Webpack. En esta etapa todavía no se genera el .sppkg.

ACTIVIDAD 10 — Comprender Gulp y Heft

**Objetivo.** Reconocer la diferencia entre el toolchain histórico
basado en Gulp y el modelo moderno utilizado por SPFx 1.23.2.

- Gulp: utilizaba tradicionalmente gulpfile.js y tareas definidas
  mediante un archivo de tareas.

- Heft: coordina tareas mediante configuración y plugins del toolchain.

- SPFx 1.22 y posteriores utilizan Heft para proyectos nuevos.

**Comprobar el proyecto actual.**

Ruta

C:\SPFx\spfx-lab2-webpart-XXX\config\rig.json

Abre rig.json y comprueba que referencia la configuración del rig de
SPFx.

**Resultado esperado.** El proyecto utiliza Heft y no contiene un
gulpfile.js generado como parte del proyecto moderno.

ACTIVIDAD 11 — Ejecutar el Web Part localmente

**Objetivo.** Levantar el servidor local y cargar el Web Part en el
SharePoint Online Workbench disponible durante el periodo de transición.

**Confiar en el certificado de desarrollo.**

Set-Location C:\SPFx\spfx-lab2-webpart-XXX

heft trust-dev-cert

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image21.png"
style="width:5.4718in;height:2.58666in" />

**Configurar el sitio de pruebas para el Workbench.**

El proyecto contiene config/serve.json con un initialPage que utiliza el
marcador {tenantDomain}. Define la variable SPFX_SERVE_TENANT_DOMAIN con
el host y la ruta del sitio, sin incluir https://.

\$env:SPFX_SERVE_TENANT_DOMAIN =
"azurenetecgp1.sharepoint.com/sites/spfxlab"

\$env:SPFX_SERVE_TENANT_DOMAIN

**Resultado esperado.** La segunda línea muestra
azurenetecgp1.sharepoint.com/sites/spfxlab.

**Iniciar Heft.**

heft start

Cuando se abra el navegador, la URL debe utilizar una sola vez el
esquema https y debe apuntar al sitio de SharePoint, por ejemplo:
https://azurenetecgp1.sharepoint.com/sites/spfxlab/\_layouts/15/workbench.aspx.

Si aparece una solicitud para permitir scripts de depuración, selecciona
Allow. Después agrega el Web Part mediante el botón + del Workbench y
verifica el título, proyecto, propietario, usuario y contador.

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image22.png"
style="width:6.9in;height:2.27986in" />

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image23.png"
style="width:6.9in;height:3.34097in" />**Resultado esperado.** El Web
Part se carga desde el servidor local y el contador aumenta al pulsar
Incrementar.

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image24.png"
style="width:6.9in;height:2.38194in" />

Nota de vigencia. El SharePoint Online Workbench está declarado como
obsoleto (deprecated) desde mayo de 2026 y tiene retiro previsto para el
1 de diciembre de 2026. Durante este módulo se utiliza porque sigue
disponible en el entorno de prueba; para nuevos escenarios de
depuración, Microsoft recomienda el SPFx Debug Toolbar.

Termina la ejecución presionando Ctrl+C en la ventana de consola.

ACTIVIDAD 12 — Inicializar Git y crear una rama de trabajo

Objetivo. Registrar el proyecto inicial y separar el trabajo del
laboratorio mediante una rama de desarrollo.

Configurar la identidad de Git

Abre .

Configura el nombre que utilizarás para identificar y el correo
electrónico asociado a tus commits:

git config --global user.name "Nombre Apellido"

git config --global user.email "correo@ejemplo.com"

Comprueba que la configuración haya quedado registrada:

git config --get user.name

git config --get user.email

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image25.png"
style="width:5.82373in;height:1.36477in" />

Inicializar el repositorio

Desde , sitúate en la carpeta raíz del proyecto e inicializa el
repositorio Git y comprueba el estado del repositorio:

Set-Location C:\SPFx\spfx-lab2-webpart-XXX

git init

git status

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image26.png"
style="width:4.74343in;height:3.65991in" />

En este momento Git debe identificar los archivos del proyecto que
todavía no han sido registrados.

Registrar el estado inicial

Agrega los archivos del proyecto al área de preparación:

git add .

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image27.png"
style="width:6.9in;height:2.85069in" />

Crea el primer commit:

git commit -m "chore: crear proyecto SPFx inicial"

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image28.png"
style="width:4.89062in;height:3.70121in" />

Comprueba nuevamente el estado:

git status

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image29.png"
style="width:3.53174in;height:0.73969in" />

Resultado esperado. Git confirma que los archivos del proyecto fueron
registrados mediante el commit inicial.

Crear la rama de trabajo

Crea una nueva rama y cambia a ella. Comprueba la rama activa:

git checkout -b feature/hello-spfx

git branch --show-current

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image30.png"
style="width:4.28185in;height:0.96889in" />

La rama feature/hello-spfx será utilizada para registrar los cambios que
realizarás durante las siguientes actividades.

ACTIVIDAD 13 — Modificar el Web Part y registrar el cambio

Objetivo. Realizar una modificación visible en el Web Part y registrar
el nuevo estado mediante un segundo commit.

Modificar el encabezado

En Visual Studio Code, abre el archivo:

src\webparts\helloSpfxXXX\components\HelloSpfxXXX.tsx

Localiza el encabezado:

\<h2\>¡Hola SPFx con React!\</h2\>

Sustitúyelo por:

\<h2\>Laboratorio 2 — SPFx + React\</h2\>

Guarda el archivo.

Comprobar el cambio con Git

Desde la terminal integrada de Visual Studio Code o desde , sitúate en
la raíz del proyecto:

Set-Location C:\SPFx\spfx-lab2-webpart-XXX

Comprueba el estado:

git status

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image31.png"
style="width:6.00084in;height:1.70857in" />

Git debe indicar que el archivo HelloSpfxXXX.tsx fue modificado.

Registrar el cambio

Agrega la modificación:

git add .

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image32.png"
style="width:6.9in;height:0.34931in" />

Crea el segundo commit:

git commit -m "feat: actualizar mensaje del Web Part"

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image33.png"
style="width:5.51119in;height:0.88554in" />

Comprueba los dos últimos commits:

git log --oneline -2

Resultado esperado. El historial muestra dos commits:

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image34.png"
style="width:5.88624in;height:0.88554in" />

- el commit inicial de creación del proyecto;

- el commit correspondiente a la actualización del mensaje del Web Part.

La rama activa continúa siendo:

feature/hello-spfx

ACTIVIDAD 14 — Generar el paquete de producción

Objetivo. Construir y empaquetar la solución SPFx para publicarla
posteriormente en el App Catalog de SharePoint.

Ejecutar el build de producción

Abre y sitúate en la raíz del proyecto. Ejecuta el build de producción:

Set-Location C:\SPFx\spfx-lab2-webpart-XXX

heft build --production

Espera a que el proceso finalice.

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image35.png"
style="width:6.9in;height:2.58333in" />

Resultado esperado. El proceso termina sin errores y muestra que el
build fue completado correctamente.

Generar el paquete .sppkg

Ejecuta:

heft package-solution --production

Este comando genera el paquete de solución que posteriormente será
cargado en el App Catalog.

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image36.png"
style="width:6.9in;height:4.17917in" />

Comprobar el paquete generado

Ejecuta:

Get-ChildItem .\sharepoint\solution\\.sppkg \| Select-Object
Name,Length,LastWriteTime

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image37.png"
style="width:6.9in;height:1.28264in" />

Identifica el archivo con extensión .sppkg.

La ruta será similar a:

C:\SPFx\spfx-lab2-webpart-XXX\sharepoint\solution\\

Resultado esperado. La carpeta sharepoint\solution contiene un archivo
.sppkg correspondiente a la solución SPFx.

Importante. Conserva este archivo. Será utilizado en la siguiente
actividad para publicar la solución en el App Catalog.

ACTIVIDAD 15 — Publicar e instalar la solución en el tenant

Objetivo. Publicar la solución SPFx en el App Catalog del tenant e
instalarla en el sitio de práctica Portal-ProyectosXXX.

Importante. En este laboratorio cuentas con permisos administrativos de
SharePoint. Utilizarás estos permisos para realizar la publicación de la
solución. Sin embargo, la solución se instalará únicamente en tu sitio
de práctica Portal-ProyectosXXX; no la habilites para todos los sitios
de la organización.

Confirmar el paquete

El paquete generado en la actividad anterior se encuentra en:

C:\SPFx\spfx-lab2-webpart-XXX\sharepoint\solution\\.sppkg

Identifica el archivo .sppkg que utilizarás para la publicación.

Abrir el App Catalog

Desde Microsoft 365, abre el Centro de administración de SharePoint.
Accede al App Catalog del tenant y localiza la biblioteca:

Apps for SharePoint

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image38.png"
style="width:6.9in;height:1.95486in" />

Nota. La ubicación exacta de algunas opciones puede variar ligeramente
según la interfaz del Centro de administración de SharePoint utilizada
por el tenant.

Cargar la solución

Carga en la biblioteca Apps for SharePoint el archivo .sppkg generado en
la Actividad 14. Cuando SharePoint solicite confirmar la implementación
de la solución, revisa la información mostrada y confirma la
publicación.

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image39.png"
style="width:3.96593in;height:4.43333in" />

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image40.png"
style="width:6.14669in;height:2.66704in" />

Espera a que SharePoint confirme que la aplicación fue agregada
correctamente al App Catalog.

Abrir el sitio de práctica

Abre el sitio:

<https://azurenetecgp1.sharepoint.com/sites/AzureNetecGP1>

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image41.png"
style="width:6.9in;height:2.18889in" />Agregar la aplicación al sitio

Dentro del portal, selecciona:

Configuración → Agregar una aplicación

Selecciona:

De su organización

Localiza la solución:

spfx-lab2-webpart-XXX

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image42.png"
style="width:6.9in;height:2.30139in" />

y selecciona Agregar. Espera a que SharePoint termine de instalar la
aplicación en el sitio.

Agregar el Web Part a una página

Abre la página de trabajo del sitio y selecciona Editar.

Selecciona:

\+ → Agregar un elemento web

Busca:

HelloSpfxXXX

Agrega el Web Part a la página.

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image43.png"
style="width:2.73177in;height:5.95833in" />

Publicar la página

Guarda o publica la página. Comprueba que el encabezado del Web Part
muestre:

Laboratorio 2 — SPFx + React

Validar el contador

Comprueba también el comportamiento del contador desarrollado en la
Actividad 8. El contador debe iniciar en:

Has hecho clic 0 veces.

Selecciona el botón: Incrementar y comprueba que el valor cambie
sucesivamente:

Has hecho clic 1 veces.

Has hecho clic 2 veces.

Has hecho clic 3 veces.

sin necesidad de recargar la página.

<img src="/mnt/data/Laboratorio2_Markdown/media/media/image44.png"
style="width:6.9in;height:3.50625in" />

Resultado esperado

- La solución:

> spfx-lab2-webpart-XXX
>
> se encuentra publicada en el App Catalog del tenant y puede agregarse
> al sitio.

- El Web Part HelloSpfxXXX se muestra correctamente y presenta:

> Laboratorio 2 — SPFx + React

- El contador funciona correctamente y aumenta en uno cada vez que se
  selecciona Incrementar.

ACTIVIDAD 16 — Comprobación final

**Objetivo.** Verificar que el ciclo completo del módulo se completó de
forma reproducible.

☐ Node.js 22.23.2 y npm disponibles.

☐ .4 o posterior disponible.

☐ Git y Visual Studio Code disponibles.

☐ PnP.PowerShell instalado y verificable.

☐ CLI for Microsoft 365 instalado y verificable.

☐ Generator SharePoint 1.23.2 instalado.

☐ Heft disponible globalmente.

☐ Proyecto creado como spfx-lab2-webpart-XXX.

☐ Web Part creado como HelloSpfxXXX.

☐ skipFeatureDeployment configurado como false.

☐ config/rig.json identificado; no se creó config/heft.json para este
laboratorio.

☐ TypeScript utilizado con una interfaz tipada.

☐ React.useState funcionando.

☐ heft build exitoso.

☐ heft trust-dev-cert ejecutado.

☐ heft start ejecutado.

☐ SPFX_SERVE_TENANT_DOMAIN configurada sin https://.

☐ Web Part ejecutado en el entorno local.

☐ Repositorio Git inicializado.

☐ Rama feature/hello-spfx creada.

☐ Commit inicial realizado.

☐ Segundo commit realizado.

☐ Build de producción exitoso.

☐ Archivo .sppkg generado.

☐ Paquete publicado en el App Catalog por el instructor.

☐ Aplicación agregada al sitio asignado.

☐ HelloSpfxXXX visible y funcional en SharePoint Online.


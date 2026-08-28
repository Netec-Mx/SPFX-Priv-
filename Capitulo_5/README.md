# Laboratorio 5

Analizador Inteligente de Proyectos

Duración: 90 minutos
 
## 1. Objetivo general

Desarrollar una solución llamada:

Analizador Inteligente de Proyectos

que permita demostrar cómo SPFx funciona como una capa de integración dentro de Microsoft 365.

La solución final tendrá cuatro capacidades:

ANALIZADOR INTELIGENTE

SHAREPOINT TEAMS POWER PLATFORM

```text
Power Apps Power Automate
```

Microsoft Graph ◄

Datos M365

Microsoft 365 Copilot

Análisis / respuestas

El objetivo coincide con el laboratorio planteado en el material: crear una solución SPFx que se empaquete para Teams, se conecte con Power Apps y Power Automate, utilice Microsoft Graph y exponga capacidades AI-first mediante Copilot Apps.

## 2. Contexto del laboratorio

En módulos anteriores, SPFx se ha utilizado principalmente para construir componentes dentro de SharePoint.


## 3. Temas del Módulo 5 cubiertos

Todos estos bloques aparecen en la estructura del documento del Módulo 5.

## 4. Resultado final esperado

Al terminar tendremos una solución conceptual como esta:

MICROSOFT TEAMS

Analizador Inteligente de Proyectos

Proyectos

Portal M365 ██████████ 100%

Migración ██████░░░░ 65%

Automatización ███░░░░░░░ 35%

[Enviar a aprobación] [Generar informe]

```text
Power Apps Power Automate
```

Gestión de proyectos Aprobaciones

Teams / Mail

SharePoint

Microsoft Graph

Microsoft 365 Copilot

```text
"¿Qué proyectos están en riesgo?"
```

0. Preparar el ambiente.

1. Crear el sitio y la lista de proyectos en SharePoint.

2. Crear el Web Part SPFx compatible con SharePoint y Teams.

3. Crear la interfaz inicial del analizador.

4. Conectar el Web Part con SharePoint REST.

5. Empaquetar la solución SPFx para Teams.

6. Publicar el paquete en el App Catalog.

7. Instalar y probar el Web Part en Teams.

8. Crear la Power App de gestión.

9. Integrar la Power App en SharePoint.

10. Crear el flujo de aprobación con Power Automate.

11. Integrar SPFx con Power Automate.

12. Integrar Microsoft Graph.

13. Comprender los permisos de Graph.

14. Revisar extensiones hacia Teams, Planner, Outlook y OneDrive.

15. Crear el componente Mi Perfil con Graph.

16. Preparar la experiencia AI-first.

17. Crear el agente Analizador Inteligente de Proyectos.

18. Definir el conocimiento del agente.

19. Definir acciones para el agente.

20. Integrar los componentes.

21. Validar el flujo de información.

22. Construir el dashboard inteligente.

23. Probar Power Apps.

24. Probar Power Automate.

25. Probar Microsoft Graph.

26. Probar la experiencia AI-first.

5. Representación general de las actividades

Nota de actualización tecnológica: el documento del Módulo 5 utiliza el flujo clásico de SPFx basado en gulp bundle --ship y gulp package-solution --ship. Ese flujo corresponde al toolchain Gulp de SPFx 1.0–1.21.1. Para proyectos nuevos con SPFx 1.22+, Microsoft utiliza actualmente Heft y Node.js 22 LTS. En este laboratorio conservaré los conceptos y objetivos del módulo, pero indicaré cuándo corresponde utilizar el flujo actual.

## ACTIVIDAD 0

Preparar las herramientas que utilizarás para desarrollar y probar la solución.

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
mkdir Modulo5
```

```text
cd Modulo5
```

## ACTIVIDAD 1

Crear SharePoint + datos de proyectos

## ACTIVIDAD 2

Crear Web Part SPFx

compatible con Teams

## ACTIVIDAD 3

Crear un dashboard inicial que después se alimentará con datos reales de SharePoint.

Abre el componente AnalizadorProyectos.tsx.

Reemplaza el contenido del renderizado principal por una interfaz que muestre:

Analizador de Proyectos

Proyectos registrados: 4

Proyectos completados: 1

Proyectos en riesgo: 1

Agrega un botón con el texto Generar informe.

Ejecuta el proyecto y verifica que el Web Part se renderice correctamente en SharePoint.

## ACTIVIDAD 4

Obtener los proyectos desde la lista Proyectos en lugar de utilizar datos escritos directamente en el código.

En el Web Part, utiliza SPHttpClient para consultar:

/_api/web/lists/getbytitle('Proyectos')/items

Construye la URL con this.context.pageContext.web.absoluteUrl.

Procesa la respuesta JSON y guarda los elementos en el estado del componente.

Recorre los elementos y muestra al menos Title, Avance y Riesgo.

Valida que aparezcan los cuatro proyectos creados en la Actividad 1.

## ACTIVIDAD 5

Preparar el Web Part para utilizarlo en SharePoint y Microsoft Teams.

Abre el manifiesto del Web Part y verifica:

```text
"supportedHosts": [
```

```text
"SharePointWebPart",
```

```text
"TeamsTab"
```

]

En config/package-solution.json revisa la configuración de la solución y conserva skipFeatureDeployment según el escenario del laboratorio.

```text
Compila:
```

```text
heft build --production
```

Genera el paquete:

```text
heft package-solution --production
```

Comprueba que exista un archivo .sppkg en sharepoint/solution/.

## ACTIVIDAD 6

Publicar el paquete .sppkg para que la solución pueda utilizarse en Microsoft 365.

Abre el SharePoint Admin Center y entra en More features > Apps > App Catalog.

Carga el archivo analizador-proyectos.sppkg.

Cuando se solicite, habilita la opción Make this solution available to Teams.

Espera a que la aplicación esté disponible antes de continuar.

## ACTIVIDAD 7

Probar que el mismo Web Part funciona dentro de Microsoft Teams.

Abre Microsoft Teams y entra en un equipo y canal de prueba.

Selecciona + para agregar una pestaña.

Busca Analizador Inteligente de Proyectos y agrégalo.

Comprueba que el dashboard se muestre como pestaña y que conserve la funcionalidad básica del Web Part.

## ACTIVIDAD 8

Crear una aplicación de negocio para consultar y modificar proyectos.

Abre Power Apps y crea una Canvas App llamada GestionProyectos.

Agrega una conexión de SharePoint al sitio Portal-Proyectos y selecciona la lista Proyectos.

Agrega una Gallery y configura Items = Proyectos.

Dentro de la galería muestra ThisItem.Title, ThisItem.Estado, ThisItem.Avance y ThisItem.Riesgo.

Agrega un Edit Form conectado a Proyectos.

Permite editar Estado, Avance, Riesgo y Descripcion.

Guarda y prueba la aplicación con uno de los proyectos.

## ACTIVIDAD 9

Integración y pruebas finales

ANALIZADOR INTELIGENTE

## ACTIVIDAD 0 — Preparación del ambiente

### Objetivo

La integración permite trabajar desde SharePoint sin duplicar la fuente de datos.

Instalar y comprobar las herramientas necesarias para desarrollar SPFx.

### 0.1. Requisitos

Se requiere:

- Windows, macOS o Linux;
- Node.js;
- npm;
- Visual Studio Code;
- Yeoman;
- generador SPFx;
- Heft para SPFx moderno;
- navegador moderno;
- tenant Microsoft 365;
- SharePoint Online;
- Microsoft Teams;
- Power Apps;
- Power Automate;
- permisos administrativos para aprobar permisos Graph;
- Microsoft 365 Copilot para la parte AI-first.
### 0.2. Instalar Node.js

Instalar Node.js 22 LTS.

Después abrir PowerShell:

```text
node --version
```

v22.x.x

Verificar npm:

```text
npm --version
```

SPFx se ejecuta sobre Node.js y utiliza npm para administrar las dependencias del proyecto.

### 0.3. Instalar Yeoman y generador SPFx

```text
Ejecutar:
```

```text
npm install @rushstack/heft yo @microsoft/generator-sharepoint --global
```

```text
Comprobar:
```

```text
yo --version
```

```text
Y:
```

```text
heft --version
```

### 0.4. Instalar Visual Studio Code

Instalar Visual Studio Code.

Después:

```text
code --version
```

Será nuestro entorno para:

editar TypeScript;

editar React;

modificar JSON;

ejecutar comandos;

revisar errores.

### 0.5. Crear carpeta de laboratorio

```text
mkdir C:\SPFx
```

```text
cd C:\SPFx
```

```text
mkdir Modulo5
```

```text
cd Modulo5
```

```text
Crear:
```

```text
C:\SPFx\Modulo5
```

### 0.6. Nota importante sobre Gulp

gulp bundle --ship

gulp package-solution --ship

porque su explicación está basada en el flujo clásico de SPFx.

```text
Actualmente:
```

SPFx 1.0 – 1.21.1

Gulp

SPFx 1.22+

Heft

Microsoft confirmó el cambio a Heft a partir de SPFx 1.22.

Por tanto, no instalaremos Gulp para un proyecto nuevo moderno, salvo que el instructor quiera reproducir exactamente el material histórico.

## ACTIVIDAD 1 — Crear la base de datos del laboratorio en SharePoint

### Objetivo

Crear la información que utilizarán SPFx, Power Apps, Power Automate y Graph.

### 1.1. Crear sitio

Crea un sitio de SharePoint llamado:

Portal-Proyectos

Por ejemplo:

/sites/PortalProyectos

### 1.2. Crear lista

```text
Crear:
```

Proyectos

```text
Columnas:
```

Para Estado:

- Pendiente
- En progreso
- Completado
Para Riesgo:

- Bajo
- Medio
- Alto
### 1.3. Agregar datos

```text
Crear:
```

Esta lista representa el origen de datos corporativos.

Después:

SharePoint List

```text
Power Apps
```

Power Automate

SPFx

## ACTIVIDAD 2 — Crear Web Part SPFx para Teams

### Objetivo

Crear un Web Part denominado:

AnalizadorProyectos

que pueda utilizarse tanto en SharePoint como en Microsoft Teams.

### 2.1. Crear proyecto

```text
Desde:
```

```text
cd C:\SPFx\Modulo5
```

```text
Ejecutar:
```

```text
yo @microsoft/sharepoint
```

### 2.2. Responder al asistente

En el asistente selecciona:

What is your solution name?

analizador-proyectos

```text
Seleccionar:
```

WebPart

```text
Framework:
```

React

```text
Nombre:
```

AnalizadorProyectos

Durante la configuración de hosts, habilitar:

SharePoint

```text
TeamsTab
```

### 2.3. Abrir proyecto

```text
code .
```

### 2.4. Instalar dependencias

```text
npm install
```

El generador crea:

package.json

con las dependencias del proyecto.

```text
npm install descarga esas dependencias.
```

### 2.5. Revisar la estructura

Tendremos algo parecido a:

analizador-proyectos/

config/

src/

webparts/

analizadorProyectos/

AnalizadorProyectos.tsx

AnalizadorProyectosWebPart.ts

components/

package.json

tsconfig.json

Finalidad

El alumno debe identificar que:

WebPart

archivo .ts

lógica SPFx

archivo .tsx

interfaz React

## ACTIVIDAD 3 — Crear la interfaz del analizador

### Objetivo

Mostrar un dashboard básico.

Analizador de Proyectos

```text
Proyectos: 4
```

```text
Completados: 1
```

En progreso: 2

Riesgo alto: 1

[ Ver proyectos ]

[ Enviar a aprobación ]

[ Generar informe ]

### 3.1. Modificar React

En el componente principal:

```text
return (
```

```text
<div>
```

```text
<h2>Analizador Inteligente de Proyectos</h2>
```

```text
<p>
```

Proyectos registrados: 4

```text
</p>
```

```text
<p>
```

Proyectos completados: 1

```text
</p>
```

```text
<p>
```

Proyectos en riesgo: 1

```text
</p>
```

```text
<button>
```

Generar informe

```text
</button>
```

```text
</div>
```

);

¿Por qué empezamos con una interfaz sencilla?

Porque primero validamos:

SPFx

React

Web Part

Después agregaremos las integraciones.

Es mejor separar:

UI

```text
de:
```

API

```text
y:
```

Automatización

para que los errores sean fáciles de identificar.

## ACTIVIDAD 4 — Conectar el Web Part con SharePoint

### Objetivo

Obtener los proyectos reales de:

Proyectos

en lugar de utilizar datos escritos directamente en el código.

### 4.1. Concepto

La arquitectura será:

React

SPFx Context

SharePoint REST

Lista Proyectos

JSON

React

### 4.2. Consultar REST

La consulta conceptual será:

/_api/web/lists/getbytitle('Proyectos')/items

Podemos utilizar el contexto SPFx para realizar la petición.

```text
Ejemplo:
```

```text
const url =
```

`${this.context.pageContext.web.absoluteUrl}` +

`/_api/web/lists/getbytitle('Proyectos')/items`;

```text
const response =
```

await this.context.spHttpClient.get(

url,

SPHttpClient.configurations.v1

);

```text
const data =
```

await response.json();

### 4.3. Mostrar resultados

```text
{items.map(item => (
```

```text
<div key={item.Id}>
```

```text
<strong>{item.Title}</strong>
```

```text
<span>
```

```text
{item.Avance}%
```

```text
</span>
```

```text
</div>
```

))}

Portal M365 100%

Migración SharePoint 65%

Automatización 35%

Intranet 10%

La idea clave de este paso es la siguiente:

Que SPFx puede funcionar como una capa intermedia:

SharePoint

SPFx

React

Esto prepara la arquitectura para incorporar después:

Graph

Power Automate

Copilot

## ACTIVIDAD 5 — Empaquetar la solución para Teams

### Objetivo

Convertir el Web Part en una aplicación que pueda agregarse como pestaña en Teams.

### 5.1. Verificar supportedHosts

Abrir el manifiesto del Web Part.

Debe existir:

```text
"supportedHosts": [
```

```text
"SharePointWebPart",
```

```text
"TeamsTab"
```

]

Microsoft también documenta actualmente TeamsTab como host para exponer un Web Part SPFx en Teams.

### 5.2. Configurar despliegue global

Para escenarios de Teams, Microsoft recomienda considerar el despliegue global de la solución; esto se controla con skipFeatureDeployment.

```text
En:
```

config/package-solution.json

```text
revisar:
```

```text
"solution": {
```

...

```text
"skipFeatureDeployment": true
```

```text
}
```

La configuración exacta puede variar según la versión generada por Yeoman.

### 5.3. Compilar

Para SPFx moderno:

```text
heft build --production
```

Si el proyecto generado incluye scripts npm equivalentes, también puede utilizarse:

```text
npm run build
```

La diferencia respecto al material original es exclusivamente el toolchain: el objetivo de generar el paquete SPFx es el mismo.

### 5.4. Empaquetar

```text
heft package-solution --production
```

Debe generarse un archivo:

sharepoint/solution/*.sppkg

## ACTIVIDAD 6 — Publicar en App Catalog

### Objetivo

Hacer disponible la solución en Microsoft 365.

### 6.1. Abrir App Catalog

Entrar al:

SharePoint Admin Center

y posteriormente:

More features

Apps

App Catalog

### 6.2. Cargar .sppkg

```text
Subir:
```

analizador-proyectos.sppkg

### 6.3. Habilitar Teams

Make this solution available to Teams

para convertir la solución en una aplicación disponible en Teams.

## ACTIVIDAD 7 — Instalar en Microsoft Teams

### Objetivo

Probar que el mismo Web Part puede funcionar dentro de Teams.

### 7.1. Abrir Teams

Ir a:

Microsoft Teams

### 7.2. Abrir un equipo y canal

Por ejemplo:

```text
Equipo:
```

Proyectos

```text
Canal:
```

General

### 7.3. Agregar pestaña

```text
Seleccionar:
```

```text
Buscar:
```

Analizador Inteligente de Proyectos

Agregarlo.

### 7.4. Resultado

```text
Teams
```

Proyectos

General

Analizador Inteligente de Proyectos

## ACTIVIDAD 8 — Crear Power App

### Objetivo

Crear una interfaz de negocio para gestionar proyectos.

### 8.1. Crear aplicación

Abrir Power Apps.

Crear una:

Canvas App

```text
Nombre:
```

GestionProyectos

### 8.2. Conectar SharePoint

Agregar conexión:

SharePoint

```text
Seleccionar:
```

Portal-Proyectos

```text
Seleccionar:
```

Proyectos

### 8.3. Crear galería

```text
Agregar:
```

Gallery

```text
Configurar:
```

Items =

Proyectos

### 8.4. Mostrar proyecto

Dentro de la galería:

ThisItem.Title

Mostrar también:

ThisItem.Estado

ThisItem.Avance

ThisItem.Riesgo

Portal M365

- Completado
100%

```text
Riesgo: Bajo
```

### 8.5. Agregar formulario

```text
Agregar:
```

Edit Form

```text
Fuente:
```

Proyectos

El usuario podrá modificar:

estado;

avance;

riesgo;

descripción.

## ACTIVIDAD 9 — Integrar Power App en SharePoint

### Objetivo

Mostrar la aplicación donde trabajan los usuarios.

Microsoft también documenta la integración de aplicaciones Canvas en SharePoint y otros servicios.

Integración en una página de SharePoint

En una página de SharePoint:

Editar página

```text
Power Apps
```

```text
Seleccionar:
```

GestionProyectos

SharePoint

Analizador SPFx

Gestión de proyectos

```text
Power Apps
```

## ACTIVIDAD 10 — Crear Power Automate

Crear un flujo de aprobación para un proyecto seleccionado.

En Power Automate crea un Instant cloud flow llamado AprobarProyecto.

Utiliza Power Apps como desencadenador.

Recibe el ID del proyecto como parámetro.

Agrega SharePoint > Get item y configura el sitio Portal-Proyectos, la lista Proyectos y el ID recibido.

Agrega Start and wait for an approval con tipo Approve/Reject.

Incluye en la solicitud Title, Avance, Riesgo y Descripcion.

Agrega una Condition sobre Outcome.

Si el resultado es Approve, actualiza Estado a Aprobado.

Si el resultado es Reject, actualiza Estado a Rechazado.

Agrega Microsoft Teams > Post a message para notificar el resultado.

## ACTIVIDAD 11 — Integrar SPFx con Power Automate

Conectar una acción del Web Part con el flujo de aprobación creado en la Actividad 10.

Utiliza el botón o acción Enviar a aprobación.

Envía el ID del proyecto seleccionado al flujo de Power Automate.

Ejecuta la acción desde el Web Part.

Comprueba que Power Automate reciba el ID, obtenga el elemento correcto y genere la aprobación.

Completa la aprobación y verifica que SharePoint y Teams reflejen el resultado.

## ACTIVIDAD 12 — Microsoft Graph

Obtener información del usuario autenticado mediante Microsoft Graph.

En config/package-solution.json agrega dentro de solution:

```text
"webApiPermissionRequests": [
```

```text
{
```

```text
"resource": "Microsoft Graph",
```

```text
"scope": "User.Read"
```

```text
}
```

]

En el Web Part importa MSGraphClientV3 desde @microsoft/sp-http.

Obtén el cliente con this.context.msGraphClientFactory.getClient('3').

Consulta /me y solicita displayName, mail y jobTitle.

Muestra esos tres valores en el Web Part.

## ACTIVIDAD 13 — Comprender permisos Graph

Verificar que los permisos declarados para Microsoft Graph sean aprobados y funcionen.

Revisa webApiPermissionRequests en config/package-solution.json.

Comprueba que el permiso User.Read esté declarado.

Publica o actualiza la solución en el App Catalog.

Solicita al administrador de Microsoft 365 que apruebe el permiso pendiente.

Vuelve a cargar el Web Part y prueba la consulta /me.

Si la consulta falla, revisa primero el permiso declarado y su consentimiento administrativo.

## ACTIVIDAD 14 — Extender Graph hacia Microsoft 365

Identificar extensiones posibles de Graph para una versión posterior de la solución.

Selecciona al menos dos escenarios que podrían aportar valor al Analizador.

Teams: canales o mensajes.

```text
Planner: tareas o progreso.
```

```text
Outlook: calendario o correo.
```

```text
OneDrive: archivos o carpetas.
```

Para cada escenario, indica qué dato necesitaría la solución y qué permiso sería necesario revisar antes de implementarlo.

## ACTIVIDAD 15 — Crear componente "Mi Perfil"

Crear el componente Mi Perfil para validar el consumo de datos de Graph.

En el componente React crea un estado para el usuario.

En useEffect obtén un MSGraphClientV3 y consulta /me.

Solicita displayName, mail y jobTitle.

Muestra la información en una sección titulada Mi Perfil.

Prueba el componente con el usuario autenticado.

## ACTIVIDAD 16 — Preparar experiencia AI-first

Definir el caso de uso AI-first que se implementará con el Analizador.

Las preguntas de prueba serán:

¿Qué proyectos están en riesgo?

¿Cuál es el proyecto con menor avance?

Resume el estado actual de los proyectos.

¿Qué proyectos deberían recibir atención prioritaria?

Define como fuente principal los datos de la lista Proyectos y como resultado una respuesta basada únicamente en información disponible.

## ACTIVIDAD 17 — Crear el agente "Analizador de Proyectos"

Crear el agente Analizador Inteligente de Proyectos.

En Visual Studio Code instala Microsoft 365 Agents Toolkit.

Abre Microsoft 365 Agents Toolkit y selecciona Create a New Agent/App.

Selecciona Declarative Agent.

Selecciona No Action para iniciar con una experiencia básica.

```text
Nombre: Analizador Inteligente de Proyectos
```

Descripción: Agente especializado en analizar proyectos, identificar riesgos y resumir avances.

Configura las instrucciones para que el agente identifique riesgos, resuma avances, identifique posibles retrasos, presente información con claridad, no invente datos y señale cuando una información no pueda determinarse.

## ACTIVIDAD 18 — Definir conocimiento del agente

Agregar el conocimiento que utilizará el agente para responder sobre proyectos.

Configura SharePoint como fuente de conocimiento.

Selecciona el sitio Portal-Proyectos y la lista Proyectos.

Prueba la pregunta: ¿Qué proyectos están en riesgo?

Comprueba que la respuesta utilice el valor Riesgo de los elementos y que no invente proyectos ni datos.

## ACTIVIDAD 19 — Crear acciones para el agente

Definir una acción del agente para iniciar el proceso de aprobación.

Documenta la acción Solicitar aprobación con esta entrada mínima:

ID del proyecto

La acción debe invocar el flujo AprobarProyecto de Power Automate.

Si el entorno del laboratorio no permite publicar la acción, deja documentada la entrada, el flujo y el resultado esperado sin simular una ejecución.

## ACTIVIDAD 20 — Integración completa

Integrar los componentes construidos y verificar que cada uno conserve su responsabilidad.

```text
SharePoint: fuente de datos de proyectos.
```

```text
SPFx: dashboard y acciones de usuario.
```

Teams: superficie de consumo.

Power Apps: gestión de proyectos.

Power Automate: aprobación y notificación.

Microsoft Graph: información del usuario.

```text
Copilot: análisis y lenguaje natural.
```

## ACTIVIDAD 21 — Crear flujo de información

Validar el flujo completo con un proyecto de prueba.

En Teams abre el Analizador Inteligente de Proyectos.

Selecciona Migración SharePoint y ejecuta Enviar a aprobación.

Comprueba que SPFx envíe el ID al flujo.

Comprueba que Power Automate consulte el proyecto y genere la aprobación.

Aprueba la solicitud.

Verifica que SharePoint cambie Estado a Aprobado y que Teams reciba la notificación.

## ACTIVIDAD 22 — Dashboard inteligente

Completar el dashboard del Web Part con información real.

Muestra el total de proyectos, completados, en progreso y de riesgo alto.

Muestra Title, Avance y Riesgo para cada proyecto.

Agrega la sección Mi Perfil con los datos obtenidos de Graph.

Agrega las acciones Enviar a aprobación y Generar informe.

Agrega un acceso a GestionProyectos.

Valida que los datos del dashboard provengan de las fuentes configuradas y no de valores fijos.

## ACTIVIDAD 23 — Prueba de Power Apps

Abre GestionProyectos.

Modifica Avance de Migración SharePoint de 65 a 70.

Guarda el registro.

Vuelve a SharePoint y verifica que Avance muestre 70.

## ACTIVIDAD 24 — Prueba de Power Automate

Selecciona Automatización.

Ejecuta Enviar a aprobación.

Completa la aprobación.

Comprueba Estado = Aprobado en SharePoint y la notificación en Teams.

## ACTIVIDAD 25 — Prueba Microsoft Graph

Abre Mi Perfil.

Verifica displayName, mail y jobTitle.

Confirma que los valores procedan de Microsoft Graph.

## ACTIVIDAD 26 — Prueba AI-first

En el agente:

Analizador Inteligente de Proyectos

Realizar las preguntas:

Pregunta 1

¿Qué proyectos están en riesgo?

Pregunta 2

¿Cuál es el proyecto con menor avance?

Pregunta 3

Resume el estado actual de los proyectos.

Pregunta 4

¿Qué proyectos deberían recibir atención prioritaria?

## 27. Flujo técnico completo

La solución final debe poder explicarse de esta forma:

MICROSOFT 365

SharePoint Teams Copilot

SPFx

```text
Power Apps Power Automate Graph
```

▼ User

Approval Teams

Planner

▼ Outlook

SharePoint OneDrive

formularios

## 28. Qué función cumple cada tecnología

## 29. Buenas prácticas

### 29.1. No colocar toda la lógica en SPFx

```text
Evitar:
```

SPFx

UI

aprobación

correos

```text
Teams
```

datos

lógica empresarial

```text
Preferir:
```

SPFx

Interfaz

Power Automate

Proceso

SharePoint

Datos

Graph

Microsoft 365

Copilot

Inteligencia

### 29.2. Utilizar Graph únicamente cuando sea necesario

Si el dato está en:

SharePoint

no necesitamos necesariamente Graph.

Graph es útil cuando necesitamos información de:

```text
Teams
```

Planner

Outlook

OneDrive

Usuarios

### 29.3. Solicitar solamente los permisos necesarios

Por ejemplo:

```text
"scope": "User.Read"
```

es preferible a solicitar permisos excesivos.

Principio

Menor privilegio

Menor superficie de riesgo

Mejor seguridad

### 29.4. Separar datos de presentación

No hacer:

```text
const proyectos = [
```

...

];

como solución definitiva.

```text
Utilizar:
```

SharePoint

API

Estado React

UI

### 29.5. No asumir que Copilot debe reemplazar la aplicación

Copilot complementa la aplicación.

```text
Ejemplo:
```

Dashboard

muestra datos

Copilot

interpreta datos

No necesariamente:

Copilot

reemplaza dashboard

## 30. Problemas frecuentes

### Problema 1 — yo @microsoft/sharepoint no funciona

```text
Comprobar:
```

```text
yo --version
```

```text
y:
```

```text
npm list -g @microsoft/generator-sharepoint
```

Si falta:

```text
npm install @microsoft/generator-sharepoint --global
```

### Problema 2 — Error de Node.js

```text
Comprobar:
```

```text
node --version
```

Para SPFx 1.22, Microsoft recomienda Node.js 22 LTS.

### Problema 3 — Se intenta ejecutar gulp

Si el proyecto es SPFx 1.22+:

gulp bundle

no es el flujo moderno.

```text
Utilizar:
```

```text
heft build --production
```

```text
y:
```

```text
heft package-solution --production
```

El motivo es el cambio de Gulp a Heft introducido en SPFx 1.22.

### Problema 4 — La aplicación no aparece en Teams

```text
Revisar:
```

supportedHosts

Debe incluir:

```text
"TeamsTab"
```

También revisar:

App Catalog

Make this solution available to Teams

### Problema 5 — Graph devuelve error de permisos

```text
Revisar:
```

package-solution.json

```text
y:
```

```text
"webApiPermissionRequests": [
```

```text
{
```

```text
"resource": "Microsoft Graph",
```

```text
"scope": "User.Read"
```

```text
}
```

]

Después verificar que el administrador haya aprobado el permiso.

### Problema 6 — Power App no carga

```text
Comprobar:
```

La aplicación fue compartida.

El usuario tiene permisos.

La conexión SharePoint funciona.

La aplicación apunta al sitio correcto.

El App ID utilizado es correcto.

### Problema 7 — Power Automate no actualiza SharePoint

```text
Revisar:
```

ID del elemento

El flujo debe recibir el ID correcto del proyecto.

## 31. Evidencias del laboratorio

### Evidencia 1 — Ambiente

```text
Captura:
```

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

### Evidencia 2 — Proyecto SPFx

Captura de:

src/

config/

package.json

### Evidencia 3 — TeamsTab

Captura del manifiesto donde aparezca:

```text
"TeamsTab"
```

### Evidencia 4 — Paquete

Captura de:

*.sppkg

### Evidencia 5 — App Catalog

Captura de la solución publicada.

### Evidencia 6 — Teams

Captura del Web Part funcionando como tab.

### Evidencia 7 — Power Apps

Captura de:

GestionProyectos

mostrando datos de SharePoint.

### Evidencia 8 — Power Automate

Captura del flujo:

```text
Power Apps
```

Get item

Approval

Condition

Update item

```text
Teams
```

### Evidencia 9 — Graph

Captura del componente:

Mi Perfil

```text
mostrando:
```

displayName

mail

jobTitle

### Evidencia 10 — Copilot

Captura de una consulta:

¿Qué proyectos están en riesgo?

y la respuesta correspondiente.

## 32. Lista de comprobación final

## 33. Resultado final del laboratorio

Al terminar el laboratorio, el alumno habrá construido una solución donde:

SHAREPOINT

Lista Proyectos

SPFx

Analizador

Inteligente

TEAMS POWER APPS

Tab Gestión

POWER AUTOMATE

Aprobaciones

Notificaciones

MICROSOFT GRAPH

Usuarios

```text
Teams
```

Planner

Outlook

OneDrive

MICROSOFT 365

COPILOT

Análisis

Resúmenes

Recomendaciones

Acciones

## 34. Qué debes poder explicar al finalizar

Teams permite llevar un Web Part SPFx a una experiencia de colaboración como una pestaña.

Power Apps permite construir interfaces y formularios que trabajan con los datos corporativos.

Power Automate permite separar la automatización de negocio del código SPFx.

Microsoft Graph permite acceder desde SPFx a información de Microsoft 365 que va más allá de SharePoint.

Copilot y los agentes declarativos permiten interpretar información y exponer experiencias de lenguaje natural.

SPFx actúa como capa de desarrollo e integración entre la experiencia de usuario y los servicios de Microsoft 365.

## 35. Resultado de aprendizaje

La evolución que debe comprender el alumno es:

Módulos anteriores

```text
"Construyo un Web Part"
```

Módulo 5

```text
"Construyo una solución integrada"
```

SharePoint

```text
Teams
```

```text
Power Apps
```

Power Automate

Microsoft Graph

Copilot

El mensaje central del Módulo 5 es precisamente que SPFx deja de verse únicamente como un framework para personalizar SharePoint y pasa a funcionar como una pieza de integración dentro de Microsoft 365. El resumen del material lo expresa como una combinación de SPFx, Power Platform, Graph y Copilot para construir aplicaciones empresariales modernas, inteligentes y utilizables en SharePoint y Teams.

Referencias oficiales utilizadas para actualizar el laboratorio

Configurar el entorno de desarrollo de SPFxConfigurar el entorno de desarrollo de SPFx — Node.js 22 LTS, Heft y Yeoman para SPFx moderno.

SPFx 1.22 y cambio a HeftSPFx 1.22 y cambio a Heft — diferencia entre el toolchain actual y el Gulp del material.

Exponer Web Parts SPFx en Microsoft TeamsExponer Web Parts SPFx en Microsoft Teams — uso de TeamsTab.

Usar Microsoft Graph en soluciones SPFxUsar Microsoft Graph en soluciones SPFx — MSGraphClientV3 y permisos Graph.

Integrar aplicaciones Canvas en SharePointIntegrar aplicaciones Canvas en SharePoint — integración de Power Apps.

Agentes declarativos para Microsoft 365 CopilotAgentes declarativos para Microsoft 365 Copilot — arquitectura actual de experiencias AI-first.

Crear agentes declarativos con Microsoft 365 Agents Toolkit — preparación de la actividad práctica de Copilot.

| Tema del módulo | Actividad |
|---|---|
| 5.1 Empaquetar soluciones SPFx para Teams | Crear Web Part compatible con Teams |
| Yeoman Generator | Preparación del proyecto |
| supportedHosts | Configuración de TeamsTab |
| Empaquetado .sppkg | Generación del paquete |
| App Catalog | Publicación |
| Teams | Instalación como tab |
| Casos de uso Teams | Dashboard de proyectos |
| 5.2 Power Apps | Crear aplicación de gestión |
| Power Apps + SharePoint | Formulario/dashboard |
| SPFx + Power Apps | Integración |
| 5.2 Power Automate | Crear flujo de aprobación/notificación |
| SPFx + Power Automate | Invocar automatización |
| Uso combinado | UI + automatización |
| 5.3 Microsoft Graph | Obtener información real del usuario |
| MSGraphClient / MSGraphClientV3 | Consumo de Graph |
| webApiPermissionRequests | Solicitud de permisos |
| /me | Datos del usuario |
| Teams / Planner / Outlook / OneDrive | Casos de extensión |
| Seguridad | Contexto del usuario y permisos |
| 5.4 Copilot Apps / AI-first | Preparar experiencia inteligente |
| SPFx | Capa de aplicación |
| Graph | Datos corporativos |
| Copilot | Interpretación |
| Análisis de proyectos | Caso práctico |
| Empaquetado y despliegue | Integración final |

| Columna | Tipo |
|---|---|
| Title | Texto |
| Responsable | Persona o texto |
| Estado | Elección |
| Avance | Número |
| FechaInicio | Fecha |
| FechaFin | Fecha |
| Riesgo | Elección |
| Descripcion | Varias líneas |

| Proyecto | Estado | Avance | Riesgo |
|---|---|---|---|
| Portal M365 | Completado | 100 | Bajo |
| Migración SharePoint | En progreso | 65 | Medio |
| Automatización | En progreso | 35 | Alto |
| Intranet | Pendiente | 10 | Bajo |

| Tecnología | Función |
|---|---|
| SharePoint | Almacena datos |
| SPFx | Capa de desarrollo e integración |
| React | Interfaz |
| Teams | Canal de consumo de la solución |
| Power Apps | Aplicación de negocio/formularios |
| Power Automate | Automatización |
| Microsoft Graph | Acceso a datos de Microsoft 365 |
| Copilot | Interacción inteligente |
| App Catalog | Distribución de SPFx |

| Actividad | Cumplido |
|---|---|
| Node.js instalado | ☐ |
| npm funcionando | ☐ |
| Visual Studio Code instalado | ☐ |
| Yeoman instalado | ☐ |
| Generador SPFx instalado | ☐ |
| Heft instalado | ☐ |
| Tenant Microsoft 365 disponible | ☐ |
| Sitio SharePoint creado | ☐ |
| Lista Proyectos creada | ☐ |
| Datos de prueba creados | ☐ |
| Web Part SPFx creado | ☐ |
| React utilizado | ☐ |
| TeamsTab configurado | ☐ |
| Web Part compilado | ☐ |
| .sppkg generado | ☐ |
| Paquete publicado en App Catalog | ☐ |
| Solución disponible en Teams | ☐ |
| Web Part instalado como tab | ☐ |
| Power App creada | ☐ |
| Power App conectada a SharePoint | ☐ |
| Power App probada | ☐ |
| Power Automate creado | ☐ |
| Aprobación implementada | ☐ |
| SharePoint actualizado por flujo | ☐ |
| Notificación Teams implementada | ☐ |
| SPFx conectado al proceso | ☐ |
| Graph configurado | ☐ |
| webApiPermissionRequests configurado | ☐ |
| User.Read aprobado | ☐ |
| /me consultado | ☐ |
| Perfil mostrado | ☐ |
| Casos Teams/Planner/Outlook comprendidos | ☐ |
| Arquitectura AI-first comprendida | ☐ |
| Agente/experiencia Copilot preparada | ☐ |
| Preguntas sobre proyectos probadas | ☐ |
| Solución integral validada | ☐ |

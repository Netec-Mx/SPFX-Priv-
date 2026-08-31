# LABORATORIO 2

Construcción y preparación del ambiente de desarrollo SPFx

Duración: 60 minutos

## 1. Objetivo

Al finalizar el laboratorio, el participante habrá preparado un ambiente local para el desarrollo de soluciones SPFx, verificando la instalación de Node.js, npm, Git y Visual Studio Code, así como de las herramientas requeridas.

También habrá creado un proyecto SPFx con React y reconocido su estructura, identificando el papel que desempeñan TypeScript y React dentro de la solución.

A partir de este proyecto, habrá utilizado un componente React con useState, comprendido el cambio del proceso de construcción de Gulp a Heft y compilado y ejecutado la solución mediante Heft. Finalmente, habrá creado un repositorio Git y una rama de desarrollo, generado el paquete .sppkg, publicado la solución en un tenant de prueba y validado el Web Part dentro de SharePoint.

## 2. Contexto

En el Laboratorio 1 el participante exploró qué es SPFx y por qué representa el modelo moderno de extensibilidad de SharePoint.

Ahora comienza el desarrollo.

El desarrollo comienza en el ambiente local, donde Node.js, npm, Git y Visual Studio Code proporcionan las herramientas necesarias para trabajar con el proyecto. Dentro del proyecto se utilizan TypeScript y React para desarrollar el Web Part, mientras Heft coordina el proceso de compilación y empaquetado. El resultado del empaquetado es un archivo .sppkg que se publica en el App Catalog para que la solución pueda utilizarse en SharePoint Online.

AMBIENTE LOCAL

- Node.js 22
- npm
- VS Code
- TypeScript
- React 17.0.1
- Heft
- Git
Proyecto SPFx

TypeScript React

- Heft
Build / Package

- .sppkg
- App Catalog
- SharePoint Online
El cambio fundamental respecto a proyectos SPFx antiguos es el toolchain basado en Heft. Microsoft indica que desde SPFx 1.22 los proyectos nuevos utilizan Heft en lugar del modelo basado en Gulp.

## 3. Al finalizar el laboratorio

Al terminar tendremos:

spfx-lab2-webpart/

- src/
- webparts/
- HelloSpfx/
- config/
- sharepoint/
- package.json
- tsconfig.json
- heft.json / configuración Heft
- README.md
Y conceptualmente:

Código

TypeScript + React

- Heft
- Build
- Package
- .sppkg
- App Catalog
- SharePoint
1. Verificar el ambiente de desarrollo.

2. Instalar las herramientas necesarias.

3. Crear el primer proyecto SPFx.

4. Explorar la estructura del proyecto.

5. Comprender el papel de TypeScript.

6. Implementar estado con React y useState.

7. Compilar el proyecto mediante Heft.

8. Comparar conceptualmente Gulp y Heft.

9. Ejecutar el Web Part localmente.

10. Inicializar Git y crear una rama de trabajo.

11. Modificar el Web Part y registrar el cambio.

12. Generar el paquete .sppkg.

13. Publicar e instalar la solución en SharePoint.

14. Validar el ciclo completo.

## 4. Actividades

## ACTIVIDAD 1

Verificación del ambiente

Esta comprobación se realiza antes de instalar herramientas para detectar incompatibilidades desde el inicio.

### Paso 1. Abrir PowerShell

Abre:

Inicio > Windows PowerShell

Ejecuta:

```powershell
node --version
```

Después:

```powershell
npm --version
```

Node debe mostrar una versión 22.x.

Para SPFx 1.23.2, Microsoft documenta Node.js v22 como versión compatible.

### Paso 2. Verificar Git

Ejecuta:

```powershell
git --version
```

Debe aparecer una versión de Git. Git permitirá registrar el estado del proyecto y posteriormente trabajar con ramas.

El módulo destaca precisamente que Git permite mantener historial, ramas y trazabilidad de cambios.

### Paso 3. Verificar Visual Studio Code

Ejecuta:

```powershell
code --version
```

Si el comando no está disponible, abre Visual Studio Code manualmente.

### Paso 4. Verificar extensiones

En VS Code verifica que estén instaladas:

- ESLint
- Prettier
- React Developer Tools
Estas extensiones forman parte del entorno propuesto en el módulo para calidad, formato y depuración.

## ACTIVIDAD 2

Las herramientas instaladas en esta actividad cumplen funciones diferentes: Heft se utiliza para construir la solución, PnP PowerShell para interactuar con SharePoint Online y el CLI para Microsoft 365 para ejecutar tareas sobre Microsoft 365.

Instalar las herramientas que se utilizarán para crear, compilar, administrar y desplegar soluciones SPFx.

Instalar las herramientas del laboratorio

El ambiente base ya contiene Node.js, Git y VS Code.

Ahora instalaremos las herramientas que el desarrollador utilizará durante el curso.

### Paso 1. Instalar Heft

Ejecuta:

```powershell
npm install -g @rushstack/heft
```

Verifica:

```powershell
heft --version
```

Heft es el orquestador moderno del proceso de build de SPFx.

### Paso 2. Instalar PnP PowerShell

Si el entorno del laboratorio todavía no lo tiene:

```powershell
Install-Module PnP.PowerShell -Scope CurrentUser
```

Verifica:

```powershell
Get-Module PnP.PowerShell -ListAvailable
```

Lo utilizaremos posteriormente para interactuar con SharePoint Online y apoyar el despliegue.

### Paso 3. Instalar CLI para Microsoft 365

Si aún no está instalado:

```powershell
npm install -g @pnp/cli-microsoft365
```

Verifica:

```powershell
m365 version
```

El CLI para Microsoft 365 proporciona comandos administrativos y de desarrollo que serán útiles para automatizar tareas sobre Microsoft 365.

Además, Microsoft documenta actualmente el uso de este CLI para analizar y actualizar proyectos SPFx hacia versiones modernas.

## ACTIVIDAD 3

Crear el primer proyecto SPFx

Crear la base de un Web Part SPFx utilizando React y TypeScript.

Sin embargo, para SPFx 1.23.2 en un curso orientado a un entorno estable, conviene documentar claramente esta diferencia:

El nuevo spfx-cli existe y Microsoft lo está posicionando como reemplazo de Yeoman.

La documentación actual todavía lo identifica como una herramienta pre-release.

Microsoft mantiene el generador de SharePoint como mecanismo de instalación para SPFx estable.

### Paso 1. Instalar Yeoman y el generador SPFx

Ejecuta y verifica:

```powershell
npm install -g yo @microsoft/generator-sharepoint
```

```powershell
yo --version
```

```powershell
yo @microsoft/sharepoint --help
```

### Paso 2. Crear carpeta de trabajo

```powershell
mkdir C:\SPFx
```

```powershell
cd C:\SPFx
```

### Paso 3. Crear el proyecto

Ejecuta:

```powershell
yo @microsoft/sharepoint
```

El asistente solicitará diferentes valores.

Solution name: spfx-lab2-webpart

Which type of client-side component: WebPart

What is your Web part name? HelloSpfx

Which template would you like to use? • React

Los demás valores pueden mantenerse con las opciones predeterminadas del laboratorio.

En este paso se ha creado una solución SPFx estructurada que integra:

No creamos todavía una aplicación completa. Creamos un proyecto SPFx estructurado que contiene:

- Configuración
- +
- TypeScript
- +
- React
- +
- SPFx
- +
- Heft
- +
- Packaging
## ACTIVIDAD 4

La estructura permite distinguir el código fuente, la configuración del proyecto y los archivos que intervienen en el empaquetado y despliegue.

Reconocer la estructura de carpetas y archivos que componen un proyecto SPFx.

Explorar la estructura del proyecto

Abre la carpeta en VS Code:

C:\SPFx\spfx-lab2-webpart

### Paso 1. Identificar las carpetas

Localiza:

- src/
- config/
- sharepoint/
### Paso 2. Identificar package.json

Abre:

package.json

Ubica:

- @microsoft/sp-core-library
- @microsoft/sp-webpart-base
- react
- react-dom
package.json contiene las dependencias que el proyecto necesita para compilar y ejecutar sus componentes.

La presencia de react y react-dom confirma que la solución utiliza React para construir la interfaz del Web Part.

### Paso 3. Identificar TypeScript

Ubica:

tsconfig.json

Este archivo contiene la configuración utilizada por TypeScript.

### Paso 4. Identificar el Web Part

Dentro de:

src/webparts/

localiza:

HelloSpfx

Dentro encontrarás los archivos TypeScript y React generados por el scaffolding.

Comprobación: identifica qué elemento contiene el código del Web Part, cuál contiene las dependencias del proyecto y cuál contiene la configuración de TypeScript.

## ACTIVIDAD 5

TypeScript como base del proyecto

La interfaz hace explícito qué propiedades puede contener un proyecto. Si se asigna un valor incompatible con el tipo declarado, TypeScript puede detectarlo durante el desarrollo o la compilación.

Reconocer cómo TypeScript define estructuras de datos y aporta tipado estático al código del Web Part.

El módulo presenta TypeScript como el lenguaje base de SPFx y destaca interfaces, tipado estático, autocompletado y orientación a objetos.

### Paso 1. Crear una interfaz

Dentro del componente React localiza el archivo .tsx.

Agrega temporalmente:

```powershell
interface IProject {
```

```powershell
id: number;
```

```powershell
name: string;
```

```powershell
owner: string;
```

```powershell
}
```

Después:

```powershell
const project: IProject = {
```

```powershell
id: 1,
```

```powershell
name: "Portal SPFx",
```

```powershell
owner: "Laboratorio"
```

```powershell
};
```

### Paso 2. Mostrar información

Puedes utilizar:

console.log(project.name);

TypeScript permite definir explícitamente la estructura esperada de un objeto.

IProject

- id: number
- name: string
- owner: string
Esto corresponde al ejemplo conceptual incluido en el módulo.

Comprobación: cambia temporalmente `id: 1` por `id: "1"` y ejecuta la compilación. Observa el error de tipos y después vuelve a dejar `id: 1`.

## ACTIVIDAD 6

React y estado

El import de React permite utilizar React.useState. useState devuelve dos valores: el estado actual y la función que debe utilizarse para actualizarlo.

Modificar el Web Part para comprender cómo React mantiene un estado y actualiza la interfaz cuando ese estado cambia.

El módulo presenta React como framework recomendado y destaca componentes reutilizables, hooks e integración con Fluent UI.

### Paso 1. Localizar el componente React

En el explorador de VS Code, abre la carpeta del Web Part y localiza el componente React generado por el scaffolding:

src/webparts/helloSpfx/components/HelloSpfx.tsx

### Paso 2. Importar React y declarar el estado

En la parte superior de HelloSpfx.tsx, junto con los demás imports, agrega o verifica:

Después, dentro del componente, declara el estado:

```powershell
const [count, setCount] = React.useState(0);
```

### Paso 3. Mostrar el valor del estado en el JSX

Dentro del método o función que devuelve el JSX del componente, agrega el siguiente párrafo en la zona donde quieres mostrar el contador:

```powershell
<p>Has hecho clic {count} veces.</p>
```

### Paso 4. Agregar el botón que modifica el estado

```powershell
<button onClick={() => setCount(count + 1)}>
```

Incrementar

</button>

El ejemplo corresponde al concepto de useState presentado en el módulo.

El estado se inicializa con el valor 0. En ese momento, el Web Part debe mostrar:

- Has hecho clic 0 veces.
Al pulsar el botón, se ejecuta setCount(count + 1). React recibe el nuevo valor de estado y vuelve a renderizar el componente para reflejar el cambio en la interfaz.

Después de un clic, el valor pasa a 1; después de dos clics, pasa a 2. El texto mostrado se actualiza porque {count} siempre representa el valor actual del estado.

No se debe modificar count directamente. La actualización debe realizarse mediante setCount(), que es la función proporcionada por useState para cambiar el estado.

La declaración const [count, setCount] = React.useState(0) crea el estado count y la función setCount. El argumento 0 establece el valor inicial.

Comprobación: ejecuta el Web Part y verifica los estados 0, 1 y 2 después de interacciones consecutivas con `Incrementar`.

## ACTIVIDAD 7

Un build exitoso indica que las herramientas del proyecto pueden procesar el código y generar los artefactos necesarios para continuar con la prueba.

Compilar el proyecto SPFx mediante Heft y comprobar que el código puede procesarse sin errores.

Compilar con Heft

Esta actividad sustituye el flujo tradicional basado en Gulp.

### Paso 1. Instalar dependencias

Desde la raíz del proyecto:

```powershell
npm install
```

### Paso 2. Ejecutar build

Ejecuta:

```powershell
heft build
```

La compilación debe terminar sin errores.

Durante la compilación, Heft coordina las herramientas configuradas para procesar el proyecto:

- TypeScript
- ESLint
- Webpack
- Build
Heft funciona como orquestador del proceso.

En esta actividad solo se construye la solución. El archivo `.sppkg` se generará posteriormente, en la Actividad 12.

## ACTIVIDAD 8

Los proyectos SPFx anteriores utilizaban un flujo de construcción basado en Gulp. Los proyectos actuales utilizan Heft como orquestador del proceso de construcción. En esta actividad compararás ambos modelos y comprobarás cuál utiliza el proyecto que acabas de crear.

Identificar las principales diferencias entre el flujo basado en Gulp y el modelo basado en Heft, y reconocer cuál utiliza el proyecto actual.

### Paso 1. Comparar los modelos

Completa la siguiente información utilizando el contenido de esta sección:

Gulp: gulpfile.js, scripts personalizados y tareas definidas mediante un archivo de tareas.

Heft: configuración del proyecto y tareas coordinadas mediante el toolchain de Heft.

### Paso 2. Revisar el proyecto

En la raíz del proyecto, localiza heft.json y package.json. Comprueba que el proyecto utiliza Heft para las tareas de construcción.

### Paso 3. Registrar las diferencias

Escribe dos diferencias concretas entre Gulp y Heft.

### Paso 4. Comprobar

Completa la afirmación: El proyecto creado en este laboratorio utiliza __________ para coordinar el proceso de construcción.

La actividad no requiere crear ni migrar un proyecto basado en Gulp.

## ACTIVIDAD 9

La prueba local permite detectar problemas de código e interfaz antes de involucrar el App Catalog o el tenant de SharePoint.

Ejecutar el Web Part en el entorno local y comprobar su comportamiento antes del despliegue.

Ejecutar el Web Part localmente

### Paso 1. Iniciar Heft

Desde la raíz:

```powershell
heft start
```

Microsoft documenta heft start para levantar el entorno local de desarrollo del Web Part.

### Paso 2. Abrir el entorno local

El proceso mostrará una dirección HTTPS local. El navegador puede mostrar una advertencia relacionada con el certificado de desarrollo.

Acepta el certificado únicamente en el ambiente de laboratorio.

### Paso 3. Probar el componente

Comprueba:

¡Hola SPFx con React!

y el contador.

Hello SPFx

Has hecho clic 0 veces.

[Incrementar]

Registra si la página local muestra el texto inicial y si el contador responde al botón. Corrige cualquier problema antes de continuar.

## ACTIVIDAD 10

Control de versiones con Git

La rama de trabajo separa los cambios del desarrollo principal y permite identificar qué modificación pertenece a esta funcionalidad.

Registrar el estado del proyecto mediante Git y crear una rama para trabajar de forma aislada.

Git permite mantener historial, ramas y colaboración, elementos fundamentales para un proyecto SPFx profesional.

### Paso 1. Inicializar Git

Desde la raíz:

```powershell
git init
```

### Paso 2. Revisar archivos

```powershell
git status
```

### Paso 3. Agregar archivos

```powershell
git add .
```

### Paso 4. Crear primer commit

```powershell
git commit -m "chore: crear proyecto SPFx inicial"
```

### Paso 5. Crear rama

```powershell
git checkout -b feature/hello-spfx
```

La rama feature/hello-spfx permite trabajar en la funcionalidad del Web Part sin modificar directamente la rama principal.

Al terminar, `git log --oneline` debe mostrar el commit inicial y la rama activa debe ser `feature/hello-spfx`.

## ACTIVIDAD 11

El segundo commit permite comprobar que Git no solo almacena el estado inicial, sino también la evolución del proyecto.

Modificar el Web Part y registrar el cambio como un nuevo commit de Git.

Realizar una modificación y versionarla

Modifica el mensaje:

¡Hola SPFx con React!

por:

Laboratorio 2 — SPFx + React

Después:

```powershell
git status
```

```powershell
git add .
```

```powershell
git commit -m "feat: actualizar mensaje del Web Part"
```

Ahora el repositorio conserva dos estados del proyecto mediante commits independientes:

- commit 1
- Proyecto inicial
- commit 2
- Web Part actualizado
Comprueba con `git log --oneline` que ahora existen dos commits y que el segundo corresponde a la modificación realizada.

## ACTIVIDAD 12

Preparar el paquete SPFx

El archivo .sppkg es el artefacto que cruza la frontera entre el desarrollo local y la instalación de la solución en SharePoint.

Generar el paquete de producción que SharePoint utilizará para instalar la solución SPFx.

### Paso 1. Build de producción

Ejecuta:

```powershell
heft build --production
```

### Paso 2. Crear el paquete

Ejecuta:

```powershell
heft package-solution --production
```

### Paso 3. Localizar el archivo

Busca:

sharepoint/solution/

Debe existir un archivo:

*.sppkg

El archivo .sppkg es el resultado empaquetado de la solución y es el archivo que se carga en el App Catalog para su instalación en SharePoint.

- Código fuente
- Build
- Package
- .sppkg
- SharePoint
Comprueba que el archivo `.sppkg` tenga una fecha y hora coherentes con la ejecución que acabas de realizar. Ese es el archivo que utilizarás en la Actividad 13.

## ACTIVIDAD 13

La solución se despliega en dos etapas: primero se publica el paquete `.sppkg` en el App Catalog; después se agrega el Web Part a una página del sitio de laboratorio. El paquete que se cargará debe ser el generado en la Actividad 12.

### Paso 1. Confirmar el paquete que se va a publicar

En el proyecto local, abre:

sharepoint/solution/

Localiza el archivo `.sppkg` generado por `heft package-solution --production`. Utiliza ese archivo; no cargues archivos `.ts`, `.tsx` ni archivos de configuración del proyecto.

### Paso 2. Abrir el App Catalog

### Paso 3. Cargar el paquete

En la biblioteca de aplicaciones del App Catalog, selecciona la opción para agregar o cargar una aplicación y selecciona el archivo `.sppkg` localizado en el Paso 1.

Cuando SharePoint muestre la solicitud de implementación, confirma la implementación y espera a que finalice antes de continuar.

### Paso 4. Abrir el sitio de laboratorio

https://<tenant>.sharepoint.com/sites/SPFx-LabSite

### Paso 5. Editar una página

Abre una página moderna que puedas modificar y selecciona Editar.

En una sección disponible, selecciona `+` para agregar contenido y elige la opción para agregar un Web Part.

### Paso 6. Buscar HelloSpfx

En el selector de Web Parts, busca `HelloSpfx` y selecciona el Web Part que corresponde a la solución publicada.

Si `HelloSpfx` no aparece, comprueba primero que el paquete correcto haya sido cargado e implementado en el App Catalog y que estás trabajando en el sitio indicado por el laboratorio.

### Paso 7. Agregar y publicar la página

Comprueba que el Web Part aparezca dentro de la sección. Guarda o publica la página según las opciones disponibles.

### Paso 8. Validar el despliegue

Comprueba que el Web Part se muestre correctamente y que el contador de la Actividad 6 funcione al seleccionar `Incrementar`. Esto confirma el funcionamiento de la solución desplegada en SharePoint.

## ACTIVIDAD 14

La validación final reúne las comprobaciones realizadas durante el laboratorio y confirma que el flujo completo puede repetirse.

Comprobar que el ambiente, el desarrollo, el versionamiento y el despliegue completaron correctamente el ciclo del laboratorio.

**Validación final**

Realiza las siguientes comprobaciones:

Ambiente

- [ ] Node.js 22.x
- [ ] npm
- [ ] Git
- [ ] VS Code
Desarrollo

- [ ] Proyecto SPFx creado
- [ ] React funcionando
- [ ] TypeScript funcionando
- [ ] useState funcionando
- [ ] Heft build exitoso
- [ ] heft start exitoso
Versionamiento

- [ ] Git inicializado
- [ ] Commit inicial
- [ ] Rama feature
- [ ] Segundo commit
Despliegue

- [ ] Build producción
- [ ] .sppkg generado
- [ ] App Catalog
- [ ] Web Part instalado
- [ ] Web Part visible en SharePoint
Comprobación final: reconstruye por escrito el recorrido desde el proyecto local hasta el Web Part visible en SharePoint e identifica el artefacto o herramienta utilizado en cada etapa.

## 5. Cierre del laboratorio

El laboratorio concluye cuando el Web Part puede compilarse, ejecutarse localmente, versionarse mediante Git, empaquetarse como .sppkg y utilizarse dentro de una página de SharePoint.

## 6. Resultado de aprendizaje del Módulo 2

Al finalizar el laboratorio, podrás explicar y ejecutar el recorrido que lleva desde un proyecto SPFx en el ambiente local hasta un Web Part disponible en SharePoint. El recorrido incluye la preparación del entorno, el desarrollo con TypeScript y React, la compilación con Heft, el control de versiones con Git, la generación del paquete .sppkg, su publicación en el App Catalog y la validación del Web Part en una página de SharePoint.

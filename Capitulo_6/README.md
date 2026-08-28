# Laboratorio 6 

Despliegue, versionado, gobernanza y monitoreo de una solución SPFx

Duración: 120 minutos

## 1. Objetivo

Al finalizar el laboratorio, el participante será capaz de:

empaquetar una solución SPFx;

generar una versión de producción mediante Heft;

publicar la solución en el App Catalog;

actualizar una solución existente mediante control de versiones;

utilizar Git/GitHub para controlar los cambios;

establecer un flujo básico de revisión y aprobación;

comprender la relación entre código fuente, paquete .sppkg y App Catalog;

consultar registros de auditoría de Microsoft 365 mediante Microsoft Purview;

identificar elementos básicos de monitoreo y seguridad asociados a una solución SPFx.

El objetivo no es solamente "subir una aplicación", sino experimentar el ciclo de vida controlado de una solución SPFx corporativa.

## 2. Contexto del laboratorio

En los módulos anteriores se desarrolló una solución SPFx que puede representar, para efectos del laboratorio, un Analizador Inteligente de Proyectos.

En este laboratorio no se evalúa únicamente la capacidad de ejecutar comandos. Cada actividad representa un control del ciclo de vida y debe dejar una evidencia que permita comprobar qué versión se preparó, cómo se construyó, dónde se publicó y cómo se investigaría una actividad posterior.

La solución será tratada ahora como si fuera una aplicación corporativa que debe pasar de desarrollo a una versión controlada y posteriormente actualizarse.

8. Monitoreo y auditoría con Microsoft Purview.

7. Uso en SharePoint / Teams.

6. Publicación mediante el App Catalog.

5. Generación del archivo .sppkg.

4. Build y package con Heft.

3. Nueva versión de la solución.

2. Cambio controlado y revisión.

1. Desarrollo y control del código con Git/GitHub.

DESARROLLO

Control de código

Git/GitHub

Cambio controlado

Nueva versión

Build con Heft

.SPPKG

App Catalog

Publicación controlada

SharePoint / Teams

Monitoreo y auditoría

Microsoft Purview

Esta secuencia recoge los conceptos centrales del módulo: desarrollo, nueva versión, build/package, App Catalog, actualización y uso por los usuarios.

## 3. Escenario práctico

La organización tiene instalada la versión:

Analizador Inteligente de Proyectos v1.0

El equipo de desarrollo debe publicar una nueva versión:

v1.1

La nueva versión tendrá un cambio sencillo de interfaz, por ejemplo:

agregar una etiqueta que indique que el componente corresponde a la versión 1.1.

No necesitamos desarrollar una funcionalidad compleja. El propósito es observar qué sucede cuando una solución existente evoluciona y debe ser publicada nuevamente.

6. La solución queda sujeta a monitoreo.

5. Se realiza la publicación controlada.

4. El paquete se publica en el App Catalog.

3. Se ejecutan el build y el package.

2. La revisión técnica valida el cambio.

1. El desarrollador implementa el cambio, crea el commit y solicita revisión.

Desarrollador

desarrolla

crea commit

solicita revisión

Revisión técnica

Build / Package

App Catalog

Publicación

Monitoreo

## 4. Preparación del ambiente

### 4.1 Software requerido

Para este laboratorio se utilizará el ambiente definido para el curso:

La compatibilidad oficial actual de SPFx 1.23.2 contempla Node.js 22 y React 17.0.1.

### 4.2 Verificar Node.js

Abrir PowerShell o una terminal de VS Code:

```text
node --version
```

Debe mostrar una versión 22.x.

Después:

```text
npm --version
```

Node.js proporciona el entorno necesario para ejecutar las herramientas de construcción de SPFx.

No debemos continuar con una versión diferente de Node.js simplemente porque "funciona": el objetivo del laboratorio es trabajar con un entorno controlado y reproducible.

## Actividad 1 — Preparar la solución

Preparar la solución SPFx que se actualizará durante el laboratorio.

Abre en Visual Studio Code la solución desarrollada en los módulos anteriores.

Desde la terminal, entra en la carpeta raíz del proyecto:

```text
cd ruta\del\proyecto
```

```text
code .
```

Comprueba que Git reconoce el repositorio:

```text
git status
```

Crea una rama específica para la actualización:

```text
git checkout -b feature/version-1.1
```

Verifica que la nueva rama esté activa:

```text
git branch
```

## Actividad 2 — Identificar y controlar la versión

Actualizar de forma coherente la versión del proyecto y la versión de la solución que se publicará.

Abre package.json y localiza:

```text
"version": "1.0.0"
```

Cambia la versión del proyecto a:

```text
"version": "1.1.0"
```

Abre config/package-solution.json y localiza solution.version.

Cambia la versión del paquete a:

```text
"version": "1.1.0.0"
```

Guarda ambos archivos y revisa que el id de la solución no cambie.

## Actividad 3 — Modificar el componente

Realizar un cambio visible y controlado en el componente React.

Abre el componente principal en:

src/webparts/analizadorInteligente/components/

Localiza el título que aparece en el Web Part.

Agrega debajo del título:

Versión 1.1

Guarda los cambios.

Ejecuta el Web Part en el entorno de prueba y comprueba que el texto aparezca correctamente.

## Actividad 4 — Registrar el cambio en Git

Revisar y registrar el cambio antes de compilar la nueva versión.

Comprueba los archivos modificados:

```text
git status
```

Inspecciona las diferencias:

```text
git diff
```

Confirma que el cambio incluye únicamente la versión y la modificación de interfaz.

Registra el cambio:

```text
git add .
```

```text
git commit -m "feat: actualiza analizador a version 1.1"
```

Verifica el historial reciente:

```text
git log --oneline -5
```

## Actividad 5 — Compilar con Heft

Compilar la solución con el toolchain moderno utilizado por SPFx 1.23.2.

Desde la raíz del proyecto ejecuta:

```text
heft build --production
```

Espera a que el proceso termine correctamente.

Si el proyecto dispone de un script npm que invoque Heft, puedes comprobar su contenido en package.json, pero para este laboratorio utiliza Heft directamente.

El resultado es una compilación preparada para generar el paquete de distribución.

## Actividad 6 — Empaquetar la solución

Generar el artefacto de despliegue correspondiente a la versión 1.1.

Ejecuta desde la raíz del proyecto:

```text
heft package-solution --production
```

Localiza el archivo generado en:

```text
sharepoint/solution/
```

Comprueba que exista el archivo .sppkg de la solución.

## Actividad 7 — Publicar en App Catalog

Publicar el paquete de la versión 1.1 en el App Catalog del tenant de laboratorio.

Accede al App Catalog de SharePoint Online.

Localiza la biblioteca donde se administran las aplicaciones.

Carga el archivo .sppkg generado en la Actividad 6.

Si SharePoint muestra información de permisos o una solicitud de implementación, revisa los datos antes de aprobar.

Comprueba que la solución quede disponible en el catálogo.

## Actividad 8 — Validar la actualización

Comprobar que la versión publicada corresponde al cambio realizado.

Abre el sitio de SharePoint donde está instalado el Web Part.

Actualiza la página.

Comprueba que el componente muestre:

Analizador Inteligente de Proyectos

Versión 1.1

Si continúa apareciendo la versión anterior, comprueba que el paquete 1.1 se haya publicado correctamente y que la solución esté actualizada en el sitio.

## Actividad 9 — Simular control de cambios corporativo

Relacionar el código revisado con la versión que se publicó.

Crea una etiqueta Git:

```text
git tag v1.1.0
```

Comprueba la etiqueta:

```text
git tag
```

Si el proyecto está conectado a GitHub, publica la rama y crea un Pull Request.

En el Pull Request indica:

Título: Actualizar Analizador Inteligente a v1.1

```text
Cambio: Actualización visual del componente.
```

Versión: 1.1.0

```text
Pruebas: Build de producción y validación en SharePoint.
```

## Actividad 10 — Definir los controles de gobernanza

Documentar el flujo de revisión y publicación de la solución.

En la raíz del repositorio crea:

CHANGE-CONTROL.md

Incluye los roles y responsabilidades:

```text
Desarrollador: implementa cambios, ejecuta pruebas y genera commits.
```

```text
Revisor: revisa código, dependencias y permisos.
```

```text
Administrador: publica en App Catalog.
```

Responsable funcional: autoriza la liberación.

Documenta el flujo:

Desarrollo → Commit → Pull Request → Revisión → Pruebas → Build → Package → App Catalog → Publicación → Monitoreo

## Actividad 11 — Simular CI/CD

Distinguir qué tareas del ciclo de publicación pueden automatizarse y cuáles requieren una decisión humana.

Crea en CHANGE-CONTROL.md dos listas.

```text
Automatizables:
```

Compilación

Pruebas

Validación técnica

Empaquetado

Generación de artefactos

Control humano:

Aprobación funcional

Revisión de permisos

Autorización de publicación

Indica qué tarea de automatización usaría Heft.

## Actividad 12 — Monitoreo y auditoría con Microsoft Purview

Consultar el registro de auditoría para identificar actividad posterior a la publicación.

Accede a Microsoft Purview con una cuenta que tenga permisos de auditoría.

Abre Audit.

Selecciona un intervalo reciente, por ejemplo Últimas 24 horas.

Cuando esté disponible, filtra por SharePoint Online.

Ejecuta la búsqueda.

Revisa al menos estos datos de un resultado:

Fecha y hora

Usuario

Servicio

Operación

Recurso

Resultado

Si la cuenta del laboratorio no tiene permisos de auditoría, documenta qué rol se necesita y qué pasos se realizarían para ejecutar la consulta.

## Actividad 13 — Relacionar seguridad y gobernanza

Relacionar cada control del ciclo de vida con la herramienta o artefacto que permite comprobarlo.

Completa esta relación en tus notas o en CHANGE-CONTROL.md:

Código fuente → Git/GitHub

Versión desplegable → App Catalog

Actividad registrada en Microsoft 365 → Microsoft Purview

Automatización de build y package → Heft

Aprobación de publicación → Flujo de gobernanza

Después, utiliza el caso de la versión 1.1 para indicar qué evidencia revisarías si se detectara una actividad inusual después de publicar.

## 18. Representación integral del laboratorio

8. Monitoreo y auditoría con Microsoft Purview y herramientas de seguridad.

7. Consumo por los usuarios.

6. Publicación hacia SharePoint y Teams.

5. App Catalog como punto de distribución.

4. Archivo .sppkg como artefacto de despliegue.

3. Heft para build y package.

2. Pull Request y revisión / QA.

1. Gobernanza y control del código mediante Git/GitHub.

GOBERNANZA

Git/GitHub

Pull Request

Revisión / QA

Heft

Build / Package

.sppkg

App Catalog

Publicación

SharePoint Teams

Usuarios

Monitoreo / Auditoría

Microsoft Purview

Defender / Security

## 19. Evidencias del laboratorio

Para evitar que el laboratorio se convierta únicamente en una secuencia de comandos, se recomienda solicitar las siguientes evidencias.

### Evidencia 1 — Control de versión

Captura o salida de:

```text
git status
```

```text
y:
```

```text
git log --oneline -5
```

### Evidencia 2 — Versión de la solución

```text
Mostrar:
```

```text
config/package-solution.json
```

```text
con:
```

```text
"version": "1.1.0.0"
```

### Evidencia 3 — Build

Mostrar ejecución exitosa de:

```text
npm run build
```

```text
o:
```

```text
heft build --production
```

### Evidencia 4 — Paquete

```text
Mostrar:
```

```text
sharepoint/solution/*.sppkg
```

### Evidencia 5 — App Catalog

Captura de la solución publicada en el App Catalog.

### Evidencia 6 — Actualización

Captura del Web Part mostrando:

Versión 1.1

### Evidencia 7 — Git

Mostrar el tag:

v1.1.0

### Evidencia 8 — Gobernanza

```text
Mostrar:
```

CHANGE-CONTROL.md

### Evidencia 9 — Auditoría

Captura de una búsqueda realizada en Microsoft Purview o, si la cuenta del laboratorio no tiene permisos, documentar el procedimiento y los permisos requeridos.

La búsqueda de auditoría en Purview permite seleccionar intervalo de fechas, servicios/cargas de trabajo y otros filtros para investigar actividades.

## 20. Resultado integral del laboratorio

Desarrollar → Versionar → Revisar → Compilar → Empaquetar → Publicar → Actualizar → Auditar → Mantener

Al finalizar el laboratorio, el participante habrá recorrido un ciclo de vida simplificado pero representativo de una solución SPFx corporativa:

DESARROLLAR

VERSIONAR

REVISAR

COMPILAR

EMPAQUETAR

PUBLICAR

ACTUALIZAR

AUDITAR

MANTENER

El resultado más importante no es solamente obtener un archivo .sppkg.

El participante debe comprender que una solución SPFx corporativa necesita un proceso que permita responder:

¿Qué versión tenemos?

¿Qué cambió?

¿Quién lo cambió?

¿Quién lo aprobó?

¿Qué se publicó?

¿Dónde está publicado?

¿Funcionó la actualización?

¿Qué ocurrió después de publicarla?

Ese es precisamente el salto conceptual entre desarrollar una solución SPFx y administrar una solución SPFx en un entorno corporativo.

| Componente | Versión / herramienta |
|---|---|
| Sistema operativo | Windows 11 |
| Node.js | LTS 22.x |
| SPFx | 1.23.2 |
| React | 17.0.1 |
| TypeScript | Compatible con SPFx 1.23.2 |
| Git | Instalado |
| Visual Studio Code | Instalado |
| Heft | Instalado durante los labs |
| Yeoman | Instalado durante los labs |
| Microsoft 365 | Tenant con SharePoint Online |
| App Catalog | Disponible |
| Microsoft Purview | Acceso de auditoría, cuando corresponda |

| Actividad | Resultado |
|---|---|
| Preparación | Ambiente validado |
| Git | Repositorio controlado |
| Rama | Cambio aislado |
| Versionado | Solución preparada como v1.1 |
| Modificación | Cambio funcional registrado |
| Commit | Cambio documentado |
| Build | Solución compilada |
| Package | .sppkg generado |
| App Catalog | Nueva versión publicada |
| Validación | Versión 1.1 funcionando |
| Tag | Código asociado a versión |
| Gobernanza | Flujo de aprobación documentado |
| CI/CD | Flujo de automatización identificado |
| Purview | Auditoría consultada |

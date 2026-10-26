# Proyecto Vue.js + Vite

Aplicación web desarrollada con **Vue.js**, utilizando **Vite** como herramienta de desarrollo y compilación, y **Axios** para realizar peticiones HTTP a servicios externos o APIs.

## Tecnologías utilizadas

- **Vue.js:** framework de JavaScript para crear interfaces web interactivas.
- **Vite:** herramienta para ejecutar el servidor de desarrollo y compilar el proyecto.
- **Axios:** biblioteca para realizar peticiones HTTP.
- **Node.js:** entorno necesario para ejecutar las herramientas del proyecto.
- **npm:** gestor de paquetes para instalar las dependencias.

## Requisitos previos

Antes de ejecutar el proyecto, es necesario tener instalado:

- Node.js, en una versión compatible con las dependencias del proyecto.
- npm, incluido con Node.js.
- Un editor de código, preferiblemente Visual Studio Code.
- Git (opcional, para clonar el repositorio).

Puedes comprobar las versiones instaladas ejecutando:

```bash
node -v
npm -v
```

## Instalación y ejecución

### 1. Descargar el proyecto

Clona el repositorio de GitHub:

```bash
git clone https://github.com/USUARIO/REPOSITORIO.git
```

Sustituye `USUARIO/REPOSITORIO` por la dirección correspondiente a este proyecto.

También puedes descargar el proyecto desde GitHub mediante **Code → Download ZIP**.

### 2. Acceder al proyecto

Abre una terminal y entra en la carpeta descargada:

```bash
cd REPOSITORIO
```

### 3. Instalar las dependencias

Ejecuta el siguiente comando para instalar las bibliotecas necesarias:

```bash
npm install
```

Esto instalará las dependencias definidas en `package.json` y generará la carpeta `node_modules`.

### 4. Ejecutar la aplicación

Para iniciar el servidor de desarrollo:

```bash
npm run dev
```

Vite mostrará en la terminal una dirección local, normalmente:

```text
http://localhost:5173/
```

Abre esa dirección en el navegador para acceder a la aplicación.

### 5. Detener el servidor

Para detener la ejecución, utiliza:

```bash
Ctrl + C
```

## Estructura del proyecto

```text
proyecto/
├── .gitignore
├── App.vue
├── README.md
├── axios.js
├── extensions.json
├── index.html
├── jsconfig.json
├── main.js
├── package-lock.json
├── package.json
├── settings.json
└── vite.config.js
```

### Descripción de los archivos principales

| Archivo | Función |
|---|---|
| `App.vue` | Componente principal de la aplicación Vue |
| `main.js` | Punto de entrada que inicializa la aplicación |
| `axios.js` | Configuración o utilización de Axios para las peticiones HTTP |
| `index.html` | Documento HTML principal |
| `package.json` | Dependencias y scripts del proyecto |
| `package-lock.json` | Registro de versiones de las dependencias |
| `vite.config.js` | Configuración de Vite |
| `jsconfig.json` | Configuración del entorno JavaScript |
| `.gitignore` | Archivos y carpetas excluidos de Git |

## Compilación para producción

Si el proyecto incluye el script `build` en `package.json`, puedes generar una versión optimizada mediante:

```bash
npm run build
```

Normalmente, Vite generará una carpeta `dist` con los archivos preparados para desplegar la aplicación.

Para visualizar la compilación, si está disponible el script correspondiente:

```bash
npm run preview
```

## Posibles errores

**Error: `npm` no se reconoce como comando**

Comprueba que Node.js y npm estén instalados correctamente.

**Error: `vite` no se reconoce**

Instala las dependencias:

```bash
npm install
```

**El puerto 5173 está ocupado**

Vite puede utilizar automáticamente otro puerto disponible. Consulta la dirección mostrada en la terminal.

**La aplicación no conecta con la API**

Comprueba la configuración de `axios.js`, la URL del servidor y que la API correspondiente esté funcionando.

## Autores

Proyecto desarrollado con Vue.js, Vite y Axios.

# LinkHub: Tu Centro de Enlaces Personal

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

---

## 1. Visión del Proyecto

Bienvenido a **LinkHub**, tu primer gran proyecto como Frontend Trainee. El objetivo de LinkHub es construir una aplicación web elegante y funcional que permita a cualquier persona crear una página de perfil simple y personalizable. En esta página, los usuarios podrán agregar, eliminar y mostrar una lista de sus enlaces más importantes: redes sociales, portafolio, blog, etc. Piensa en ello como una tarjeta de presentación digital, un "Linktree" personal que construiremos desde cero.

Este proyecto está diseñado para ser tu campo de entrenamiento. A lo largo de 15 días, te enfrentarás a tareas diarias que te introducirán, paso a paso, en los conceptos fundamentales y avanzados del desarrollo web moderno con React.

### Características Principales que Construiremos:

*   **Vista de Perfil Pública:** Una página principal que muestra la foto de perfil, nombre de usuario, una breve biografía y la lista de enlaces.
*   **Lista de Enlaces Dinámica:** Los enlaces se cargarán y se mostrarán desde un estado centralizado.
*   **Panel de Administración:** Una ruta privada (`/admin`) donde podrás añadir nuevos enlaces y eliminar los existentes.
*   **Navegación Multi-página (SPA):** Usaremos un enrutador para navegar entre la vista pública y el panel de admin sin recargar la página.
*   **Persistencia de Datos:** Los enlaces se guardarán en el navegador para que no se pierdan al recargar la página.

---

## 2. Pila Tecnológica y Conceptos Fundamentales

No solo construiremos una aplicación, sino que entenderemos a fondo las herramientas que la hacen posible.

### JavaScript Moderno (ES6+)

La base de todo. React utiliza las características más modernas de JavaScript para hacer el código más conciso y potente.
*   **Funciones de Flecha (`=>`):** Una sintaxis más corta para escribir funciones.
*   **Desestructuración (`const { a, b } = obj`):** Para extraer fácilmente datos de objetos y arrays.
*   **Módulos (`import`/`export`):** Para organizar nuestro código en archivos reutilizables.
*   **Operador de Propagación (`...`):** Fundamental para trabajar con arrays y objetos de forma inmutable en React.

### React (La Biblioteca de UI)

React no es un framework, es una *biblioteca*. Su único trabajo es ayudarte a construir interfaces de usuario interactivas de manera eficiente.

*   **Filosofía de Componentes:** La idea central de React. Dividimos la UI en piezas pequeñas, aisladas y reutilizables llamadas componentes. Un botón, un formulario, una tarjeta de perfil... todo es un componente. Luego, los "componemos" para construir pantallas completas.
*   **JSX (JavaScript XML):** Es lo que nos permite escribir una sintaxis similar a HTML dentro de nuestro JavaScript. No es HTML, pero nos ayuda a visualizar la estructura de nuestra UI. En el fondo, cada etiqueta JSX se transforma en una llamada a una función de React.
*   **El DOM Virtual:** Esta es la "magia" de React. En lugar de manipular directamente el lento DOM del navegador, React mantiene una copia ligera en memoria (el DOM Virtual). Cuando el estado de un componente cambia, React calcula la forma más eficiente de actualizar el DOM real, haciendo que las actualizaciones sean increíblemente rápidas.
*   **Hooks (`useState`, `useEffect`...):** Son funciones especiales que nos permiten "engancharnos" a las características de React desde nuestros componentes funcionales. `useState` nos da memoria (estado), y `useEffect` nos permite ejecutar código en respuesta a eventos del ciclo de vida del componente (como cuando se monta por primera vez o cuando una prop cambia).

### Vite (La Herramienta de Construcción)

Vite es nuestro taller de desarrollo. Es un servidor de desarrollo local y una herramienta para empaquetar nuestro código para producción.

*   **Desarrollo Ultrarrápido:** A diferencia de herramientas más antiguas, Vite utiliza los módulos ES nativos del navegador para servir el código. Esto significa que el servidor de desarrollo arranca casi instantáneamente y las actualizaciones (Hot Module Replacement - HMR) son rapidísimas.
*   **Configuración Mínima:** Viene pre-configurado para React, por lo que podemos empezar a programar en segundos sin pelearnos con archivos de configuración.
*   **Build Optimizado:** Cuando estamos listos para desplegar, `npm run build` crea una versión súper optimizada de nuestro código en la carpeta `dist`.

### React Router DOM (El Enrutador)

Para que nuestra aplicación se sienta como un sitio web con múltiples páginas, usamos React Router. Nos permite sincronizar la UI con la URL del navegador.

*   **Single Page Application (SPA):** React Router nos ayuda a crear SPAs. El usuario navega por diferentes "páginas", pero el `index.html` nunca se recarga. Esto proporciona una experiencia de usuario fluida y rápida, similar a la de una aplicación de escritorio.

---

## 3. Estructura del Proyecto

Esta es la estructura de carpetas que construiremos. Entenderla te ayudará a navegar el proyecto.

```
linkhub-project/
|-- public/             # Archivos estáticos (imágenes, fuentes, favicons, db.json)
|-- src/                # ¡Aquí vivimos el 99% del tiempo!
|   |-- assets/         # Archivos CSS, SVGs que se importan en componentes.
|   |-- components/     # Componentes de UI pequeños y reutilizables (Button, Card, Input).
|   |-- context/        # (Días avanzados) Para manejar estado global.
|   |-- data/           # Datos iniciales o mockeados.
|   |-- pages/          # Componentes que representan una página o ruta completa (HomePage, AdminPage).
|   |-- App.jsx         # Componente raíz que contiene la lógica principal y el enrutador.
|   |-- main.jsx        # El punto de entrada de la aplicación. Aquí se inicia React.
|-- .gitignore          # Archivos y carpetas que Git debe ignorar.
|-- package.json        # Define el proyecto, sus dependencias y scripts.
|-- README.md           # ¡Este archivo!
```

---

## 4. Cómo Empezar

Sigue estos pasos para poner en marcha el entorno de desarrollo en tu máquina.

1.  **Prerrequisitos:** Asegúrate de tener [Node.js](https://nodejs.org/) (que incluye npm) instalado en tu sistema.

2.  **Clonar el Proyecto (si estuviera en Git):**
    ```bash
    git clone <URL_DEL_REPOSITORIO>
    cd linkhub-project
    ```

3.  **Instalar Dependencias:**
    Este comando lee el `package.json` e instala todas las librerías necesarias (React, Vite, etc.).
    ```bash
    npm install
    ```

4.  **Ejecutar el Servidor de Desarrollo:**
    ¡Esto inicia la magia! Tu aplicación estará disponible localmente.
    ```bash
    npm run dev
    ```
    Abre tu navegador y visita la URL que aparece en la terminal (usualmente `http://localhost:5173`).

5.  **Construir para Producción:**
    Cuando quieras crear la versión final para subir a un servidor, ejecuta:
    ```bash
    npm run build
    ```
    Esto generará la carpeta `dist` con todo lo necesario.

---

## 5. Tu Rol como Trainee

Tu misión es simple: completa una tarea cada día. En la carpeta `/tareas` encontrarás un archivo `.md` por cada día de trabajo. Cada archivo contiene:

*   El **objetivo** del día.
*   Una **explicación teórica** del concepto clave que aprenderás.
*   Un **dibujo conceptual** en ASCII para ayudarte a visualizar la idea.
*   La **tarea** con instrucciones paso a paso y fragmentos de código.

¡No te apresures! Lo importante es que entiendas el *porqué* de cada paso. Si te atascas, no dudes en preguntar. ¡Estoy aquí para guiarte!

---

## 6. Alcance del Proyecto: Un MVP Frontend con Backend Simulado

Es crucial entender qué habrás construido al final de estos 15 días. Tendrás en tus manos un **MVP (Producto Mínimo Viable) de frontend, completo y funcional.**

### ¿Qué significa esto?

Significa que has construido la parte de la aplicación con la que el usuario interactúa directamente. Desde la perspectiva de alguien que usa la aplicación en su navegador, el proyecto es 100% funcional: puede ver perfiles, navegar entre páginas, añadir y eliminar enlaces, y los datos se guardan entre sesiones. Has completado un proyecto de frontend de principio a fin.

### El Backend Simulado

La parte del "backend" (servidor, base de datos, API) ha sido **simulada** a propósito. Este es un punto clave y una práctica profesional estándar.

*   **Un Backend Real** vive en un servidor en internet, gestiona una base de datos central y expone una API para que el frontend le pida datos.
*   **Nuestro Backend Simulado** evolucionó durante el curso:
    1.  **Estado de React:** Los datos vivían en la memoria del componente.
    2.  **`localStorage`:** Los datos se guardaban en el navegador del usuario.
    3.  **`db.json` y `fetch`:** La simulación más realista. Imitamos una llamada a una API, pero en lugar de contactar a un servidor, leímos un archivo local.

### ¿Por Qué Este Enfoque?

1.  **Enfoque en el Aprendizaje:** Como Frontend Trainee, tu objetivo es dominar React y el ecosistema del lado del cliente. Mezclar el aprendizaje del backend desde cero al mismo tiempo puede ser abrumador y contraproducente.
2.  **Simulación Profesional:** En los equipos de desarrollo del mundo real, el frontend y el backend suelen trabajar en paralelo. Es extremadamente común que el equipo de frontend cree un "backend mock" o simulado (como nuestro `db.json`) para poder construir y probar toda la interfaz de usuario sin tener que esperar a que la API real esté lista.

Al completar este proyecto, no solo has aprendido React, sino que has trabajado de una manera que refleja los flujos de trabajo profesionales. Estás perfectamente preparado para tomar la interfaz que has construido y conectarla a cualquier API de backend real, que sería el siguiente paso lógico en tu camino como desarrollador.
# Día 10: Refactorizando para el Futuro y Estructura del Proyecto

## Objetivos del Día

-   Preparar la aplicación para obtener datos de una fuente externa (API).
-   Crear una estructura de carpetas más escalable (`services`).
-   Simular una llamada a una API usando `setTimeout`.

## Tareas

### 1. ¿Por qué refactorizar ahora?

Nuestra aplicación funciona, pero todos nuestros datos están "hardcodeados" (escritos directamente en el código) dentro del componente `App.jsx`. En una aplicación real, estos datos vendrían de una base de datos a través de una API (Interfaz de Programación de Aplicaciones).

Antes de hacer una llamada a una API real, vamos a organizar nuestro código de una manera que facilite la integración de esa lógica. La idea es separar completamente la lógica de obtención de datos de nuestros componentes de React.

### 2. Creando una capa de `services`

Es una buena práctica crear una carpeta `services` (o `api`) en `src`. Esta carpeta contendrá archivos responsables de toda la comunicación con APIs externas. Los componentes no sabrán *cómo* se obtienen los datos (si es de una API REST, GraphQL, etc.), solo llamarán a una función de un servicio y esperarán los datos.

**Paso a paso:**

1.  Dentro de la carpeta `src`, crea una nueva carpeta llamada `services`.
2.  Dentro de `services`, crea un nuevo archivo llamado `linkService.js`.

**¿Por qué y qué hace?**

*   **`src/services`**: Centraliza la lógica de comunicación externa. Si en el futuro cambias de API, solo tendrás que modificar los archivos dentro de esta carpeta, y no tus componentes.
*   **`linkService.js`**: Este archivo contendrá todas las funciones relacionadas con la obtención y manipulación de los datos de los enlaces, como `getLinks`, `createLink`, `deleteLink`, etc.

### 3. Simulando una API en `linkService.js`

Todavía no tenemos un backend, así que vamos a simular uno. Crearemos una función `getLinks` que devuelve nuestros datos hardcodeados después de un pequeño retraso, para imitar la asincronía de una llamada de red real.

**Paso a paso:**

1.  Abre `src/services/linkService.js`.
2.  Mueve el array de datos de `App.jsx` a este archivo.
3.  Crea una función `getLinks` que devuelva los datos dentro de una `Promise` que se resuelve después de 1 segundo.

**Código para `linkService.js`:**

```javascript
// Datos iniciales que simulan nuestra base de datos
const initialLinks = [
  { id: 1, title: 'Mi Portafolio', url: 'https://github.com/johndoe' },
  { id: 2, title: 'Twitter', url: 'https://twitter.com/johndoe' },
  { id: 3, title: 'LinkedIn', url: 'https://linkedin.com/in/johndoe' },
];

// Función que simula una llamada a una API para obtener los enlaces
export const getLinks = () => {
  return new Promise((resolve) => {
    // Simulamos un retraso de red de 1 segundo (1000 milisegundos)
    setTimeout(() => {
      resolve(initialLinks);
    }, 1000);
  });
};
```

**¿Por qué y qué hace?**

*   **`initialLinks`**: Hemos movido los datos fuera de nuestro componente. Ahora viven en nuestra capa de servicio.
*   **`export const getLinks = () => { ... }`**: Definimos y exportamos la función `getLinks` para que pueda ser usada en otros archivos (como `App.jsx`).
*   **`return new Promise(...)`**: Las operaciones de red en JavaScript son asíncronas. Las `Promises` son objetos que representan la eventual finalización (o fallo) de una operación asíncrona. Nuestra función `getLinks` devuelve inmediatamente una `Promise`.
*   **`setTimeout(() => { ... }, 1000)`**: Esta función de JavaScript ejecuta el código que se le pasa después de un número específico de milisegundos. La usamos para simular que la red tarda 1 segundo en responder.
*   **`resolve(initialLinks)`**: Después de 1 segundo, llamamos a `resolve`. Esto "cumple" la `Promise` y le entrega los datos (`initialLinks`) a quien sea que la estuviera esperando.

### 4. Preparando `App.jsx` para la carga de datos

Ahora que tenemos nuestro servicio, vamos a modificar `App.jsx` para que empiece con una lista de enlaces vacía. La llenaremos en la siguiente tarea usando `useEffect` y nuestro nuevo servicio.

**Paso a paso:**

1.  Abre `src/App.jsx`.
2.  Modifica la llamada a `useState` para que el estado inicial de `links` sea un array vacío `[]`.

**Código a modificar en `App.jsx`:**

```jsx
function App() {
  // El estado ahora comienza vacío. Los datos se cargarán de forma asíncrona.
  const [links, setLinks] = useState([]);

  // ... el resto del componente ...
}
```

**¿Por qué y qué hace?**

*   **`useState([])`**: Cuando la aplicación se cargue por primera vez, no habrá enlaces. Esto es lo que vería un usuario real mientras espera que los datos lleguen del servidor. La aplicación se renderizará inicialmente con una lista vacía.

Con esta refactorización, hemos desacoplado nuestros datos de la interfaz de usuario y hemos preparado el terreno para una de las tareas más comunes y cruciales en el desarrollo de aplicaciones web: obtener datos de una API. Hemos creado una estructura de proyecto más robusta y profesional que nos servirá bien a medida que la aplicación crezca en complejidad.
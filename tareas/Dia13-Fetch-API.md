# Día 13: Obteniendo Datos Reales con `fetch`

## Objetivos del Día

-   Aprender a usar la API `fetch` del navegador para hacer peticiones HTTP GET.
-   Interactuar con una API pública real ([JSONPlaceholder](https://jsonplaceholder.typicode.com/)) para obtener datos.
-   Adaptar nuestro `linkService` y nuestro componente para manejar la estructura de datos de la API real.

## Tareas

### 1. ¿Qué es `fetch`?

`fetch` es una API moderna, nativa de los navegadores, que nos permite realizar peticiones de red (HTTP). Es la sucesora de `XMLHttpRequest` y está basada en `Promises`, lo que la hace muy fácil de usar en nuestro flujo asíncrono.

La sintaxis básica para una petición GET es:

```javascript
fetch('https://api.example.com/data')
  .then(response => response.json()) // Parsea la respuesta a JSON
  .then(data => {
    console.log(data); // Usa los datos
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

Una cosa importante a tener en cuenta es que `fetch` tiene un manejo de errores en dos pasos:
1.  El primer `.then()` se ejecuta incluso si la respuesta es un error HTTP como 404 (No Encontrado) o 500 (Error del Servidor). Debemos comprobar manualmente si la respuesta fue exitosa.
2.  El `.catch()` solo se activa si hay un error de red que impide que la petición se complete (ej: el usuario está offline).

### 2. Usando una API de Ejemplo: JSONPlaceholder

Para no tener que construir nuestro propio backend todavía, usaremos [JSONPlaceholder](https://jsonplaceholder.typicode.com/), un servicio gratuito que proporciona una API REST de ejemplo, perfecta para prototipado y tutoriales.

Vamos a usar el endpoint `https://jsonplaceholder.typicode.com/users/1/todos`, que nos dará una lista de tareas (todos) para un usuario específico. Usaremos estos "todos" como si fueran nuestros enlaces.

### 3. Actualizando el `linkService.js`

Vamos a reemplazar nuestra simulación con una llamada `fetch` real.

**Paso a paso:**

1.  Abre `src/services/linkService.js`.
2.  Reescribe la función `getLinks` para que use `fetch`.

**Código a modificar en `linkService.js`:**

```javascript
// Ya no necesitamos los datos hardcodeados
// const initialLinks = [...];

// Función que llama a la API real
export const getLinks = () => {
  console.log('Fetching data from API...');
  return fetch('https://jsonplaceholder.typicode.com/users/1/todos')
    .then(response => {
      // Comprobamos si la respuesta de la red fue exitosa
      if (!response.ok) {
        // Si no lo fue, lanzamos un error para que lo capture el .catch()
        throw new Error('Network response was not ok');
      }
      // Si la respuesta es exitosa, la parseamos de JSON a un objeto JS
      return response.json();
    });
};
```

**¿Por qué y qué hace?**

*   Hemos eliminado nuestra `Promise` manual y `setTimeout`. Ahora `fetch` nos devuelve una `Promise` directamente.
*   **`if (!response.ok)`**: La propiedad `ok` de la respuesta es un booleano que es `true` si el código de estado HTTP está en el rango 200-299 (éxito). Si no es `ok`, lanzamos un `Error`. Esto detendrá la ejecución del `.then()` y pasará el control al bloque `.catch()` en el lugar donde se llamó a `getLinks` (en `ProfilePage.jsx`).
*   **`return response.json()`**: El cuerpo de una respuesta `fetch` es un "stream". El método `.json()` lee este stream hasta el final y lo parsea como JSON. Es importante destacar que `.json()` también es una operación asíncrona y devuelve su propia `Promise`, pero la sintaxis de encadenamiento de `.then()` lo maneja por nosotros de forma transparente.

### 4. Adaptando el Componente a los Nuevos Datos

La API de JSONPlaceholder nos devuelve objetos con una estructura diferente a la que usábamos. Un "todo" de la API se ve así:

```json
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

No tenemos una propiedad `url`. Para que nuestra aplicación funcione, necesitamos adaptar los datos recibidos a la estructura que nuestros componentes esperan (`{ id, title, url }`).

**Paso a paso:**

1.  Abre `src/pages/ProfilePage.jsx` (el antiguo `App.jsx`).
2.  Modifica el `.then()` en el `useEffect` de carga para transformar los datos recibidos.

**Código a modificar en `ProfilePage.jsx`:**

```jsx
// ... en el useEffect de carga ...
getLinks()
  .then(apiTodos => {
    // 2. Transformar los datos de la API a nuestra estructura interna
    console.log('Datos recibidos de la API:', apiTodos);
    const adaptedLinks = apiTodos.map(todo => ({
      id: todo.id,
      title: todo.title, // La API ya nos da un título
      // Como la API no nos da una URL, creamos una de ejemplo
      url: `https://jsonplaceholder.typicode.com/todos/${todo.id}`
    }));
    console.log('Datos adaptados para nuestra app:', adaptedLinks);

    setLinks(adaptedLinks);
    setError(null);
  })
// ... el resto del código ...
```

**¿Por qué y qué hace?**

*   **`apiTodos.map(todo => ({...}))`**: Estamos usando `.map()` para crear un **nuevo** array. Por cada `todo` que recibimos de la API, creamos un nuevo objeto con la estructura que nuestros componentes `LinkCard` y `LinkList` esperan.
*   Esto se conoce como una **capa de adaptación** o **anti-corrupción**. Es una práctica excelente. Significa que el resto de nuestra aplicación no necesita saber nada sobre la estructura de la API externa. Si la API cambia en el futuro (por ejemplo, `title` pasa a llamarse `name`), solo tendríamos que cambiarlo en este único lugar, y no en todos nuestros componentes.

Ahora, cuando recargues la página, verás que los datos se cargan desde una API real en Internet. Has dado un paso gigante, pasando de una aplicación con datos de juguete a una que puede consumir cualquier fuente de datos externa.
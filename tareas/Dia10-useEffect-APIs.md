# Día 10: Usando `useEffect` para Cargar Datos de API

## Objetivos del Día

-   Usar el Hook `useEffect` para realizar una llamada a nuestra API simulada cuando el componente se monta.
-   Actualizar el estado del componente con los datos recibidos de la API.
-   Entender el patrón de carga de datos asíncronos en React.

## Tareas

### 1. El `useEffect` para la Carga Inicial

Recordemos cómo funciona `useEffect`:

> `useEffect(setupFunction, dependencies)`

Para cargar datos cuando un componente aparece por primera vez, queremos usar el `useEffect` que se ejecuta **solo una vez**. Esto se logra pasando un **array de dependencias vacío `[]`**.

Esto le dice a React: "Ejecuta esta función de `setup` solo después del primer renderizado, y no la vuelvas a ejecutar nunca más, porque no depende de ninguna `prop` o `estado` que pueda cambiar".

### 2. Implementando la Carga de Datos en `App.jsx`

Vamos a usar `useEffect` para llamar a nuestra función `getLinks` del servicio que creamos y luego usaremos `setLinks` para guardar los datos devueltos en el estado.

**Paso a paso:**

1.  Abre `src/App.jsx`.
2.  Importa la función `getLinks` de tu servicio.
3.  Dentro de la función `App`, utiliza un `useEffect` con un array de dependencias vacío para llamar a `getLinks`.

**Código a modificar en `App.jsx`:**

```jsx
import React, { useState, useEffect } from 'react';
// ... otras importaciones ...
// 2. Importar la función del servicio
import { getLinks } from './services/linkService';

function App() {
  const [links, setLinks] = useState([]);

  // 3. useEffect para cargar los datos iniciales
  useEffect(() => {
    console.log("Componente montado. Obteniendo enlaces...");
    // Llamamos a la función asíncrona
    getLinks()
      .then(initialLinks => {
        // Cuando la Promise se resuelve, actualizamos el estado
        console.log("Enlaces recibidos:", initialLinks);
        setLinks(initialLinks);
      })
      .catch(error => {
        // Es una buena práctica manejar posibles errores
        console.error("Error al obtener los enlaces:", error);
      });
  }, []); // <-- El array de dependencias vacío es crucial

  // ... el resto de la lógica y el JSX ...
}
```

**¿Por qué y qué hace?**

*   **`import { getLinks } ...`**: Importamos la función que creamos para poder usarla.
*   **`useEffect(() => { ... }, [])`**: Le decimos a React que ejecute el código de adentro solo una vez, justo después de que `App.jsx` se muestre en pantalla por primera vez.
*   **`getLinks()`**: Llamamos a nuestra función asíncrona. Esta función devuelve una `Promise` inmediatamente.
*   **`.then(initialLinks => { ... })`**: El método `.then()` se adjunta a la `Promise`. El código dentro del `.then()` no se ejecuta inmediatamente. Espera a que la `Promise` se "resuelva". En nuestro `linkService`, esto sucede después de 1 segundo cuando llamamos a `resolve(initialLinks)`.
*   **`setLinks(initialLinks)`**: Una vez que los datos han llegado, llamamos a `setLinks` para actualizar nuestro estado. Esto provoca un **segundo renderizado** del componente `App`. En el primer renderizado, `links` era un array vacío (`[]`). En este segundo renderizado, `links` contendrá los datos de nuestra API simulada, y el componente `LinkList` los mostrará en pantalla.
*   **`.catch(error => { ... })`**: Si por alguna razón la `Promise` fallara (por ejemplo, si la API no estuviera disponible y usáramos `reject()` en nuestro servicio), el código dentro del `.catch()` se ejecutaría. Siempre es importante pensar en los posibles errores.

### 3. El Flujo Completo

Veamos qué sucede cuando cargas la página:

1.  **Primer Renderizado**: El componente `App` se ejecuta. El estado `links` es `[]`. React renderiza la página sin ningún enlace. Verás el encabezado del perfil y el formulario, pero la lista de enlaces estará vacía.
2.  **Ejecución de `useEffect`**: Inmediatamente después del renderizado, React ve el `useEffect`. Como es la primera vez, ejecuta la función de `setup`.
3.  **Llamada a la API**: Se llama a `getLinks()`. Nuestra API simulada empieza a "esperar" durante 1 segundo.
4.  **Espera Asíncrona**: Durante ese segundo, la aplicación está completamente funcional. No está bloqueada.
5.  **Resolución de la `Promise`**: Después de 1 segundo, la `Promise` se resuelve y el código dentro del `.then()` se ejecuta.
6.  **Actualización de Estado**: Se llama a `setLinks()` con los datos de los enlaces.
7.  **Segundo Renderizado**: React detecta el cambio de estado y vuelve a renderizar el componente `App`. Esta vez, la variable `links` contiene los datos. El componente `LinkList` recibe estos datos y los renderiza en la pantalla.
8.  **UI Actualizada**: Los enlaces aparecen en la página.

¡Felicidades! Has implementado con éxito el patrón de carga de datos más común en las aplicaciones de React. Este conocimiento es la base para construir aplicaciones dinámicas y conectadas a datos del mundo real.

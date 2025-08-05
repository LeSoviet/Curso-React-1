# Día 9: Efectos Secundarios con `useEffect`

## Objetivos del Día

-   Entender qué es un "efecto secundario" en el contexto de React.
-   Aprender a usar el Hook `useEffect` para ejecutar código en respuesta a cambios en el ciclo de vida del componente.
-   Mostrar un mensaje en la consola cada vez que la lista de enlaces cambie.

## Tareas

### 1. ¿Qué es un Efecto Secundario?

En React, el trabajo principal de un componente es calcular y devolver JSX para renderizar la UI. Todo lo demás se considera un **efecto secundario**. 

Un efecto secundario es cualquier operación que interactúa con el "mundo exterior" fuera del flujo normal de renderizado de React. Algunos ejemplos comunes son:

-   Hacer una petición a una API para obtener datos.
-   Configurar una suscripción a un evento (como `window.addEventListener`).
-   Manipular el DOM directamente (aunque esto se hace raramente en React).
-   **Guardar datos en `localStorage` o `sessionStorage`.**
-   **Actualizar el título del documento (`document.title`).**
-   Registrar mensajes en la consola.

El problema es: ¿**cuándo** ejecutamos este código? No podemos ponerlo directamente en el cuerpo del componente, porque se ejecutaría en cada renderizado, lo cual puede ser ineficiente o incorrecto. Necesitamos una forma de decirle a React: "Ejecuta este código solo después de que el componente se haya renderizado, o solo cuando una pieza de estado específica haya cambiado".

Para esto, usamos el Hook `useEffect`.

### 2. El Hook `useEffect`

El Hook `useEffect` te permite sincronizar tu componente con un sistema externo. Acepta dos argumentos:

1.  Una **función de configuración (setup)**: El código que quieres ejecutar. Este es tu "efecto".
2.  Un **array de dependencias (opcional)**: Una lista de valores (props o estado) de los que depende tu efecto.

```jsx
useEffect(() => {
  // 1. Función de configuración (tu efecto)
  console.log('El efecto se ha ejecutado');

  // Opcional: Función de limpieza
  return () => {
    console.log('Limpiando el efecto anterior');
  };
}, [/* 2. Array de dependencias */]);
```

### 3. El Array de Dependencias: La Clave de `useEffect`

El comportamiento de `useEffect` cambia drásticamente según lo que pongas en el array de dependencias. Esta es la parte más importante que hay que entender:

-   **Si omites el array de dependencias por completo:**
    ```jsx
    useEffect(() => { /* ... */ });
    ```
    El efecto se ejecutará **después de cada renderizado** del componente. (Úsalo con cuidado).

-   **Si pasas un array vacío `[]`:**
    ```jsx
    useEffect(() => { /* ... */ }, []);
    ```
    El efecto se ejecutará **solo una vez**, después del primer renderizado del componente. Es perfecto para inicializaciones, como hacer una llamada a una API para obtener los datos iniciales.

-   **Si pasas valores en el array `[prop, estado]`:**
    ```jsx
    useEffect(() => { /* ... */ }, [links, user]);
    ```
    El efecto se ejecutará **una vez después del primer renderizado** Y **cada vez que cualquiera de los valores en el array de dependencias cambie**. Si en un nuevo renderizado, `links` o `user` son diferentes al renderizado anterior, el efecto se volverá a ejecutar.

### 4. Implementar nuestro primer `useEffect`

Vamos a usar `useEffect` para un ejemplo sencillo: mostraremos un mensaje en la consola cada vez que nuestra lista de enlaces (`links`) cambie.

**Paso a paso:**

1.  Abre `src/App.jsx`.
2.  Importa `useEffect` desde `react`, junto a `useState`.
3.  Dentro de la función `App`, añade una llamada a `useEffect`.

**Código a añadir/modificar en `App.jsx`:**

```jsx
// 2. Importar useEffect
import React, { useState, useEffect } from 'react';
import './App.css';
import 'bootstrap/dist/css/bootstrap.min.css';
import LinkList from './components/LinkList';
import AddLinkForm from './components/AddLinkForm';

function App() {
  const [links, setLinks] = useState([/*...*/]);

  // ... funciones handleAddLink y handleDeleteLink ...

  // 3. Añadir el useEffect
  useEffect(() => {
    // Este código se ejecutará cada vez que el estado 'links' cambie.
    console.log('La lista de enlaces ha cambiado:', links);
    document.title = `LinkHub (${links.length} enlaces)`;
  }, [links]); // El array de dependencias

  return (
    <div className="container mt-5">
      {/* ... JSX ... */}
    </div>
  );
}

export default App;
```

**¿Por qué y qué hace?**

*   **`useEffect(() => { ... }, [links])`**: Le estamos diciendo a React:
    1.  "Después de que te renderices, ejecuta este código".
    2.  "Y vigila la variable de estado `links`. Si en un futuro renderizado, el array `links` ha cambiado, vuelve a ejecutar este código".
*   **`console.log(...)`**: Simplemente muestra un mensaje en la consola del navegador. Abre las herramientas de desarrollador (F12) y mira la pestaña "Consola" cuando añadas o elimines un enlace.
*   **`document.title = ...`**: Este es un efecto secundario clásico. Estamos interactuando con el DOM del navegador (fuera de React) para cambiar el título de la pestaña del navegador. Ahora, el título reflejará dinámicamente cuántos enlaces tienes.

Aunque este ejemplo es sencillo, has aprendido la herramienta fundamental que te permitirá conectar tus aplicaciones React con el mundo exterior, ya sea para obtener datos de un servidor, guardar en el almacenamiento local o cualquier otra interacción que necesites realizar.
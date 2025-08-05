# Día 9: Efectos Secundarios con `useEffect`

## Objetivo del Día

Aprender a ejecutar "efectos secundarios" en tus componentes. Un efecto secundario es cualquier código que interactúa con algo fuera de React, como cargar datos, suscripciones, o manipular el DOM directamente. Usaremos `useEffect` para cambiar el título de la página del navegador.

## Concepto Teórico

El hook `useEffect` nos permite ejecutar una función después de que el componente se haya renderizado.

`useEffect(funcionDeEfecto, [dependencias]);`

-   **`funcionDeEfecto`**: El código que quieres ejecutar.
-   **`[dependencias]`**: Un array opcional de variables. El efecto se volverá a ejecutar *solo si* alguna de estas variables ha cambiado desde el último renderizado.
    -   Si omites el array (`useEffect(fn)`): El efecto se ejecuta en **cada** renderizado. (¡Cuidado con los bucles infinitos!)
    -   Si pasas un array vacío (`useEffect(fn, [])`): El efecto se ejecuta **solo una vez**, después del primer renderizado. Ideal para buscar datos iniciales.
    -   Si pasas variables (`useEffect(fn, [a, b])`): El efecto se ejecuta la primera vez, y luego cada vez que `a` o `b` cambien.

## Dibujo Conceptual

`useEffect` es como darle a tu componente instrucciones sobre qué hacer *después* de que ha aparecido en la pantalla.

```
      Proceso de Renderizado de React
      +------------------------------------+
      | 1. El estado o las props cambian   |
      | 2. React calcula la nueva UI       |
      | 3. React actualiza el DOM (pinta)  |
      +-----------------+------------------+
                        |
                        v
      +-----------------+------------------+
      |      useEffect se ejecuta          |
      | (si sus dependencias cambiaron)    |
      |                                    |
      | - Cambiar título del documento     |
      | - Pedir datos a una API            |
      | - ... etc                          |
      +------------------------------------+
```

## Tarea

Usaremos `useEffect` para dos cosas:
1.  Establecer el título de la página la primera vez que se carga.
2.  Actualizar el título de la página cada vez que el número de enlaces cambie.

### Paso a Paso

1.  **Modificar `App.jsx`:**
    *   Importa `useEffect` desde React: `import { useEffect } from 'react';`.
    *   Añade un `useEffect` que se ejecute cuando el componente se monta por primera vez y cada vez que la longitud de `links` cambie.

    ```jsx
    // src/App.jsx
    import { useState, useEffect } from 'react'; // <-- Importar useEffect
    // ...

    function App() {
      const [links, setLinks] = useState([ ... ]);
      const profileData = {
        username: '@tu-usuario',
        // ...
      };
      // ...

      // Este efecto se ejecutará después del primer renderizado
      // y cada vez que el array 'links' cambie.
      useEffect(() => {
        document.title = `${profileData.username} | (${links.length}) Enlaces`;
        console.log('Efecto ejecutado: El título de la página ha cambiado.');
      }, [links, profileData.username]); // <-- Array de dependencias

      // ... el resto del componente (addLink, return, etc.)
      return (
        <div className="App">
          {/* ... */}
        </div>
      );
    }

    export default App;
    ```

2.  **Verifica el Resultado:**
    *   Carga la aplicación. Mira la pestaña del navegador. Debería mostrar tu nombre de usuario y el número de enlaces (ej: "@tu-usuario | (3) Enlaces").
    *   Añade un nuevo enlace usando el formulario.
    *   Observa cómo el título en la pestaña del navegador se actualiza instantáneamente para reflejar el nuevo total de enlaces.
    *   Revisa la consola para ver el mensaje del `useEffect`.

`useEffect` es un hook muy potente y versátil. Lo usarás constantemente para integrar tus componentes de React con el mundo exterior, especialmente para obtener datos de servidores.

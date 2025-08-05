# Día 4: Introducción al Estado con `useState`

## Objetivo del Día

Hoy introduciremos uno de los conceptos más poderosos de React: el **estado**. El estado permite que tus componentes "recuerden" información y reaccionen a eventos. Usaremos el hook `useState` para manejar datos que cambian con el tiempo.

## Tarea

Vamos a hacer que nuestra lista de enlaces sea interactiva. Añadirás un botón que, al hacer clic, agregará un nuevo enlace a la lista. Esto te enseñará cómo el estado de un componente puede ser modificado y cómo React actualiza la UI automáticamente.

### Paso a Paso

1.  **Importar `useState`:**
    *   En `src/App.jsx`, importa el hook `useState` desde React en la primera línea.

    ```javascript
    import { useState } from 'react';
    ```

2.  **Inicializar el Estado:**
    *   Dentro del componente `App`, elimina el array constante `linksData`.
    *   En su lugar, llama a `useState` para crear una variable de estado. `useState` devuelve un array con dos elementos: el valor actual del estado y una función para actualizarlo. Usaremos la desestructuración de arrays para obtenerlos.
    *   Inicializaremos el estado con el mismo array de enlaces que teníamos antes.

    ```jsx
    // src/App.jsx

    function App() {
      const initialLinks = [
        { id: 1, title: 'Mi Portafolio', url: 'https://github.com' },
        { id: 2, title: 'Mi LinkedIn', url: 'https://linkedin.com' },
        { id: 3, title: 'Mi Twitter', url: 'https://twitter.com' }
      ];

      const [links, setLinks] = useState(initialLinks);

      // ... resto del componente
    }
    ```

3.  **Crear la Función para Añadir Enlaces:**
    *   Aún en `App.jsx`, crea una función llamada `handleAddLink`. Esta función será la encargada de actualizar el estado.
    *   Dentro de `handleAddLink`, crea un objeto para el nuevo enlace. Asegúrate de que tenga un `id` único (por ahora, podemos usar `Date.now()` para generar uno simple).
    *   Llama a `setLinks` para actualizar el estado. Para añadir el nuevo enlace sin borrar los existentes, crearemos un nuevo array que contenga todos los `...links` antiguos más el `newLink`.

    ```jsx
    // src/App.jsx
    // ...después de la inicialización del estado

    const handleAddLink = () => {
      const newLink = {
        id: Date.now(),
        title: 'Nuevo Enlace',
        url: '#'
      };
      setLinks([...links, newLink]);
    };
    ```

4.  **Añadir el Botón en la UI:**
    *   En el `return` de `App.jsx`, añade un botón. Dale un `onClick` que llame a la función `handleAddLink` que acabas de crear.

    ```jsx
    // src/App.jsx
    // ...en el return

    return (
      <div className="App">
        {/* ... ProfileHeader ... */}
        <LinkList links={links} />
        <button onClick={handleAddLink}>Añadir Nuevo Enlace</button>
      </div>
    );
    ```

5.  **Ver la Magia:**
    *   Ve a tu navegador. Verás la lista inicial.
    *   Haz clic en el botón "Añadir Nuevo Enlace". ¡Boom! Un nuevo enlace aparece en la lista. React ha detectado el cambio de estado y ha vuelto a renderizar el componente `App` (y sus hijos) con los nuevos datos.

Has dado tu primer paso en la creación de aplicaciones dinámicas e interactivas. ¡Felicidades!
# Día 10: `useEffect` para Efectos Secundarios

## Objetivo del Día

Hoy conocerás otro hook fundamental: `useEffect`. Este hook te permite ejecutar "efectos secundarios" en tus componentes. Los efectos secundarios son operaciones que no están relacionadas con el renderizado en sí, como peticiones a una API, suscripciones, o manipular el DOM directamente.

## Tarea

Vamos a simular la carga de datos desde una API. Usarás `useEffect` para cargar la lista inicial de enlaces de forma asíncrona cuando el componente `App` se monta por primera vez. También, cambiaremos el título de la pestaña del navegador para que muestre el nombre de usuario.

### Paso a Paso

1.  **Simular una API:**
    *   Para no depender de un servidor real, crearemos una función que simule una petición de red. Devuelve una Promesa que se resuelve después de un tiempo con nuestros datos.
    *   En `src/App.jsx`, fuera del componente `App`, añade esta función:

    ```javascript
    const mockFetchLinks = () => {
      const initialLinks = [
        { id: 1, title: 'Mi Portafolio', url: 'https://github.com' },
        { id: 2, title: 'Mi LinkedIn', url: 'https://linkedin.com' },
        { id: 3, title: 'Mi Twitter', url: 'https://twitter.com' }
      ];
      return new Promise(resolve => {
        setTimeout(() => {
          resolve(initialLinks);
        }, 1000); // Simula un retraso de 1 segundo
      });
    };
    ```

2.  **Usar `useEffect` para Cargar Datos:**
    *   Importa `useEffect` de React en `App.jsx`.
    *   Inicializa el estado de `links` con un array vacío: `useState([])`.
    *   Llama a `useEffect`. Le pasarás una función y un array de dependencias.
    *   La función que le pases a `useEffect` llamará a nuestra `mockFetchLinks` y actualizará el estado con los datos recibidos.
    *   El **array de dependencias** (el segundo argumento) es crucial. Si le pasas un array vacío `[]`, el efecto se ejecutará **solo una vez**, después del primer renderizado. Esto es perfecto para cargar datos iniciales.

    ```jsx
    // src/App.jsx
    import { useState, useEffect } from 'react';

    function App() {
      const [links, setLinks] = useState([]);
      // ... otros estados y datos de perfil

      useEffect(() => {
        // El efecto a ejecutar
        mockFetchLinks().then(data => {
          setLinks(data);
        });
      }, []); // El array de dependencias vacío

      // ... resto del componente
    }
    ```

3.  **Usar `useEffect` para Actualizar el Título del Documento:**
    *   Puedes tener múltiples `useEffect` en un componente.
    *   Añade otro `useEffect` que se ejecute cada vez que el nombre de usuario cambie. Para ello, pondremos `profileData.username` en el array de dependencias.

    ```jsx
    // src/App.jsx
    // ... dentro de App()

    const profileData = { /* ... */ };

    useEffect(() => {
      document.title = `${profileData.username} | LinkHub`;
    }, [profileData.username]); // Se ejecuta si username cambia
    ```

4.  **Observar el Comportamiento:**
    *   Abre la aplicación. Notarás que la lista de enlaces está vacía durante un segundo y luego aparece. ¡Estás viendo la carga asíncrona en acción!
    *   Mira la pestaña del navegador. Debería mostrar el nombre de usuario que definiste en `profileData`.

Has aprendido a usar `useEffect` para manejar operaciones asíncronas y otros efectos secundarios, un paso esencial para conectar tu aplicación de React con el mundo exterior (APIs, etc.).
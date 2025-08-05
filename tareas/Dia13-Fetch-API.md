# Día 13: Obteniendo Datos de una "API" con `fetch`

## Objetivo del Día

Simular la carga de datos desde un servidor externo. Reemplazaremos nuestros datos iniciales "hardcodeados" con datos cargados desde un archivo JSON local usando la `fetch` API del navegador dentro de un `useEffect`.

## Concepto Teórico

En aplicaciones reales, los datos casi siempre vienen de un servidor a través de una API. El hook `useEffect` con un array de dependencias vacío `[]` es el lugar perfecto para hacer estas llamadas a la red, ya que se ejecuta solo una vez, cuando el componente se monta.

La `fetch` API es una herramienta moderna del navegador para hacer peticiones de red. Devuelve una "Promesa" (`Promise`), que es un objeto que representa la eventual finalización (o fallo) de una operación asíncrona.

El flujo básico con `async/await` (una sintaxis más limpia para manejar promesas) es:
1.  `async function fetchData() { ... }`
2.  `const response = await fetch('url');` // Espera la respuesta
3.  `const data = await response.json();` // Espera a que el cuerpo se convierta a JSON
4.  `setData(data);` // Actualiza el estado con los datos recibidos

## Dibujo Conceptual

Tu aplicación hace una petición a una fuente de datos externa para poblar su estado inicial.

```
      +--------------------------------+
      |           Componente           |
      |                                |
      | useEffect(..., []) se dispara  |
      |           |                    |
      |           v                    |
      |      async function fetch...() |
      +---------------+----------------+
                      |
                      v (Petición de Red)
      +---------------+----------------+
      |      Servidor / Archivo JSON   |
      +---------------+----------------+
                      |
                      v (Respuesta con Datos)
      +---------------+----------------+
      | .then(data => setData(data))   |
      |      Actualiza el estado       |
      +--------------------------------+
```

## Tarea

1.  Crear un archivo `db.json` para simular nuestra base de datos.
2.  Modificar `App.jsx` para que cargue los datos desde este archivo al iniciar.

### Paso a Paso

1.  **Crear `db.json`:**
    *   En la carpeta `public` de tu proyecto Vite, crea un archivo `db.json`. (Ponerlo en `public` hace que sea accesible directamente desde el navegador).
    *   Copia tus datos iniciales a este archivo con formato JSON.

    ```json
    // public/db.json
    {
      "profile": {
        "imageUrl": "https://via.placeholder.com/150",
        "username": "@json-user",
        "bio": "Mis datos vienen de un archivo JSON."
      },
      "links": [
        { "id": 1, "title": "Vite Docs", "url": "https://vitejs.dev" },
        { "id": 2, "title": "React Docs", "url": "https://react.dev" }
      ]
    }
    ```

2.  **Modificar `App.jsx` para usar `fetch`:**
    *   Usa un `useEffect` para cargar los datos cuando la aplicación se inicie.
    *   Inicializa los estados como vacíos o nulos.

    ```jsx
    // src/App.jsx
    import { useState, useEffect } from 'react';
    // ...

    function App() {
      const [profile, setProfile] = useState(null);
      const [links, setLinks] = useState([]);

      useEffect(() => {
        // Definimos una función asíncrona dentro del efecto
        const fetchData = async () => {
          try {
            const response = await fetch('/db.json'); // Vite sirve public/ como /
            const data = await response.json();
            setProfile(data.profile);
            setLinks(data.links);
            console.log('Datos cargados desde db.json!', data);
          } catch (error) {
            console.error('Error al cargar los datos:', error);
          }
        };

        fetchData(); // La llamamos
      }, []); // <-- Array vacío para que se ejecute solo una vez

      // ... lógica de addLink y deleteLink ...

      // ¡Importante! Manejar el caso donde los datos aún no han cargado
      if (!profile) {
        return <div>Cargando perfil...</div>;
      }

      return (
        <div className="App">
          {/* ... */}
        </div>
      );
    }
    ```

### Verifica el Resultado

Al recargar la página, deberías ver brevemente el mensaje "Cargando perfil..." y luego aparecerá la información cargada desde `db.json`. Has hecho tu aplicación mucho más realista, separando los datos de la lógica de la vista.

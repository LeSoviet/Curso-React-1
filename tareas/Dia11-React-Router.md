# Día 11: Navegación con React Router

## Objetivos del Día

-   Entender qué es una Aplicación de Página Única (SPA) y cómo funciona el enrutamiento del lado del cliente.
-   Instalar y configurar la librería `react-router-dom`.
-   Crear un sistema de enrutamiento básico con una página de inicio y una futura página de administración.

## Tareas

### 1. ¿Qué es el Enrutamiento del Lado del Cliente?

Hasta ahora, nuestra aplicación vive en una sola URL (por ejemplo, `localhost:5173`). Una **Aplicación de Página Única (SPA)** es una aplicación web que no necesita recargar la página desde el servidor para navegar a diferentes secciones. 

En lugar de que el servidor envíe una nueva página HTML cada vez que haces clic en un enlace, una librería de **enrutamiento del lado del cliente** como React Router intercepta estos clics. Luego, cambia la URL en la barra de direcciones del navegador y renderiza diferentes componentes de React en la misma página, creando la ilusión de que estás navegando entre diferentes páginas.

### 2. Instalar React Router

Vamos a añadir la librería a nuestro proyecto.

**Paso a paso:**

1.  Abre la terminal de tu editor de código.
2.  Asegúrate de que estás en la carpeta raíz de tu proyecto (`linkhub-project`).
3.  Ejecuta el siguiente comando:

    ```bash
    npm install react-router-dom
    ```

**¿Por qué y qué hace?**

*   **`npm install react-router-dom`**: Este comando descarga e instala el paquete `react-router-dom`, que contiene los Hooks y componentes necesarios para manejar el enrutamiento en una aplicación web de React.

### 3. Configurar el Enrutador

La configuración de React Router se realiza típicamente en el punto de entrada de la aplicación, `src/main.jsx`.

**Paso a paso:**

1.  Abre `src/main.jsx`.
2.  Importa los componentes necesarios de `react-router-dom`: `createBrowserRouter` y `RouterProvider`.
3.  Define tus rutas usando `createBrowserRouter`.
4.  Envuelve tu componente `<App />` con el `RouterProvider`.

**Código a modificar en `src/main.jsx`:**

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App.jsx';
import './index.css';
import 'bootstrap/dist/css/bootstrap.min.css';

// 2. Importar componentes de React Router
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

// 3. Definir las rutas
const router = createBrowserRouter([
  {
    path: '/', // La ruta raíz (página de inicio)
    element: <App />,
    // En el futuro, aquí podríamos añadir una página de error
    // errorElement: <ErrorPage />,
  },
  {
    path: '/admin', // Una nueva ruta para la página de administración
    // Todavía no hemos creado el componente, así que por ahora ponemos un placeholder
    element: <div>Página de Administración</div>,
  },
]);

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    {/* 4. Usar el RouterProvider en lugar de App directamente */}
    <RouterProvider router={router} />
  </React.StrictMode>,
);
```

**¿Por qué y qué hace?**

*   **`createBrowserRouter(routes)`**: Esta función toma un array de objetos de ruta y crea una instancia del enrutador.
*   **Objeto de Ruta (`{ path, element }`)**: Cada objeto define una ruta.
    *   `path`: La URL que activará esta ruta.
    *   `element`: El componente de React que se debe renderizar cuando la URL coincide con el `path`.
*   **`<RouterProvider router={router} />`**: Este componente, que ahora envuelve nuestra aplicación, es el que hace toda la magia. Proporciona el contexto de enrutamiento a toda la aplicación, permitiendo que otros componentes y Hooks de React Router funcionen correctamente.

### 4. Modificar `App.jsx` para la Página Principal

Ahora que `App.jsx` es solo una de las posibles páginas (la que se muestra en la ruta `/`), podemos pensar en ella como nuestro `HomePage` o `ProfilePage`.

Por ahora, no necesitamos hacer ningún cambio en `App.jsx`. Sigue siendo el componente que muestra el perfil y la lista de enlaces.

### 5. Añadir Enlaces de Navegación

Para poder movernos entre nuestras nuevas "páginas", necesitamos enlaces. React Router proporciona un componente `<Link>` para esto.

**Paso a paso:**

1.  Abre `src/App.jsx`.
2.  Importa el componente `Link` de `react-router-dom`.
3.  Añade un enlace de navegación en algún lugar de la página, por ejemplo, en el encabezado.

**Código a añadir/modificar en `App.jsx`:**

```jsx
import React, { useState, useEffect } from 'react';
import { Link } from 'react-router-dom'; // 2. Importar Link
// ... otras importaciones ...

function App() {
  // ... estados y lógica ...

  return (
    <div className="container mt-5">
      {/* 3. Añadir un enlace de navegación */}
      <nav className="d-flex justify-content-end mb-4">
        <Link to="/admin" className="btn btn-secondary">Ir a Admin</Link>
      </nav>

      <ProfileHeader {...} />
      <AddLinkForm {...} />
      {/* ... renderizado condicional ... */}
    </div>
  );
}
```

**¿Por qué y qué hace?**

*   **`import { Link } ...`**: Importamos el componente `Link`.
*   **`<Link to="/admin">`**: Este componente es similar a una etiqueta `<a>` de HTML, pero con una diferencia crucial: **no recarga la página**. En su lugar, le dice a React Router que actualice la URL y renderice el componente asociado a la ruta `/admin`.
*   **`to="/admin"`**: La prop `to` es como el `href` de una etiqueta `<a>`. Especifica la ruta de destino.

Ahora, cuando hagas clic en el botón "Ir a Admin", la URL cambiará a `/admin` y verás el mensaje "Página de Administración" sin que la página se recargue. Has convertido tu aplicación de una sola vista en una verdadera Aplicación de Página Única (SPA), sentando las bases para añadir funcionalidades más complejas en diferentes secciones.
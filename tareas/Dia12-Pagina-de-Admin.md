# Día 12: Creando la Página de Administración y Layouts de Ruta

## Objetivos del Día

-   Crear un nuevo componente de página: `AdminPage.jsx`.
-   Reestructurar las rutas para que `App.jsx` actúe como un "layout" o "shell" para otras páginas.
-   Usar el componente `<Outlet>` de React Router para renderizar rutas anidadas.

## Tareas

### 1. El Problema: Repetición de UI

Actualmente, `App.jsx` contiene toda nuestra UI principal. Si creamos una `AdminPage` separada, ¿cómo compartimos elementos comunes como la barra de navegación, el encabezado o el pie de página entre `App.jsx` (nuestra página de perfil) y `AdminPage.jsx`? No queremos copiar y pegar estos elementos en cada nueva página que creemos.

La solución es usar **rutas anidadas**. La idea es tener un componente de "layout" principal que defina la estructura común (como la navegación) y luego renderice los componentes de página específicos (perfil, admin, etc.) dentro de él.

Nuestro `App.jsx` es el candidato perfecto para convertirse en este componente de layout.

### 2. Crear el Componente `AdminPage.jsx`

Primero, vamos a crear el componente para nuestra nueva página.

**Paso a paso:**

1.  Crea una nueva carpeta `src/pages` para organizar nuestros componentes de página, para no mezclarlos con los componentes reutilizables.
2.  Mueve `App.jsx` a `src/pages` y renómbralo a `ProfilePage.jsx`. **(¡Importante! Tendrás que actualizar las rutas de importación en otros archivos después de esto)**.
3.  Dentro de `src/pages`, crea un nuevo archivo llamado `AdminPage.jsx`.

**Código para `AdminPage.jsx`:**

```jsx
import React from 'react';

function AdminPage() {
  return (
    <div>
      <h1 className="mb-4">Panel de Administración</h1>
      <p>
        Aquí es donde podrías gestionar tus enlaces, ver estadísticas o configurar tu perfil.
      </p>
      {/* En el futuro, aquí podrías tener tablas, formularios, etc. */}
    </div>
  );
}

export default AdminPage;
```

**¿Por qué y qué hace?**

*   **`src/pages`**: Creamos esta carpeta para diferenciar claramente entre componentes reutilizables (`src/components`) y componentes que representan una página o vista completa.
*   **`AdminPage.jsx`**: Por ahora, es un componente simple que solo muestra un título y un texto. En el futuro, podría contener su propia lógica y estado para gestionar los datos de la aplicación.

### 3. Crear el Componente de Layout (`App.jsx`)

Ahora, vamos a crear un nuevo `App.jsx` que servirá como el layout principal.

**Paso a paso:**

1.  Crea un nuevo archivo `src/App.jsx` (o renombra el antiguo si no lo has movido).
2.  Importa el componente `<Outlet>` y `<Link>` de `react-router-dom`.
3.  Define la estructura común de la UI (navegación, contenedor principal) y usa `<Outlet />` como marcador de posición para donde se renderizarán las páginas anidadas.

**Código para el nuevo `src/App.jsx` (Layout):**

```jsx
import React from 'react';
import { Outlet, Link } from 'react-router-dom';

function App() {
  return (
    <div className="container mt-5">
      <header className="d-flex justify-content-between align-items-center mb-4">
        <h1 className="h3">LinkHub</h1>
        <nav>
          <Link to="/" className="btn btn-outline-primary me-2">Mi Perfil</Link>
          <Link to="/admin" className="btn btn-outline-secondary">Admin</Link>
        </nav>
      </header>

      <main>
        {/* Las rutas anidadas se renderizarán aquí */}
        <Outlet />
      </main>

      <footer className="text-center text-muted mt-5">
        <p>&copy; 2025 LinkHub</p>
      </footer>
    </div>
  );
}

export default App;
```

**¿Por qué y qué hace?**

*   **`<Outlet />`**: Este es el componente clave de React Router para layouts. Actúa como un **marcador de posición**. React Router mirará la URL actual y renderizará el componente de la ruta hija que coincida en el lugar donde se encuentra el `<Outlet />`.
*   Hemos movido la navegación a este layout principal. Ahora estará visible en todas las páginas que anidemos dentro de él.

### 4. Reconfigurar las Rutas en `main.jsx`

Finalmente, ajustamos nuestra configuración de rutas para reflejar esta nueva estructura anidada.

**Paso a paso:**

1.  Abre `src/main.jsx`.
2.  Importa los nuevos componentes de página (`ProfilePage` y `AdminPage`).
3.  Modifica la estructura del enrutador para que `App` sea la ruta padre y las páginas sean sus `children`.

**Código a modificar en `src/main.jsx`:**

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App.jsx'; // Este es nuestro nuevo Layout
import ProfilePage from './pages/ProfilePage.jsx'; // La antigua App.jsx
import AdminPage from './pages/AdminPage.jsx';
import './index.css';
import 'bootstrap/dist/css/bootstrap.min.css';

import { createBrowserRouter, RouterProvider } from 'react-router-dom';

const router = createBrowserRouter([
  {
    path: '/', // La ruta padre
    element: <App />, // Renderiza siempre el Layout
    // errorElement: <ErrorPage />,
    children: [ // Rutas anidadas que se renderizarán en el Outlet
      {
        index: true, // Esta es la ruta por defecto para '/'
        element: <ProfilePage />,
      },
      {
        path: 'admin', // Se renderiza en '/admin'
        element: <AdminPage />,
      },
    ],
  },
]);

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>,
);
```

**¿Por qué y qué hace?**

*   **`element: <App />`**: La ruta padre (`/`) ahora renderiza nuestro componente `App` (el layout).
*   **`children: [...]`**: Esta propiedad contiene un array de rutas anidadas.
*   **`index: true`**: La ruta con `index: true` es la que se renderiza en el `<Outlet>` cuando la URL coincide exactamente con la del padre (`/`). Por lo tanto, al ir a la raíz de nuestro sitio, veremos el `ProfilePage`.
*   **`path: 'admin'`**: Esta ruta no tiene un `/` al principio. Es una ruta **relativa** a su padre. React Router la combinará para formar la URL completa `/admin`. Cuando naveguemos a `/admin`, el `AdminPage` se renderizará en el `<Outlet>`.

Ahora tienes una estructura de enrutamiento mucho más potente y escalable. Puedes seguir añadiendo nuevas páginas como `children` del layout principal, y todas compartirán la misma navegación y estructura sin duplicar código.
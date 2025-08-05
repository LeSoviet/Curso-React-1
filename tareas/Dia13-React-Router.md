# Día 13: Rutas Dinámicas y Parámetros de URL

## Objetivos del Día

-   Entender qué son las rutas dinámicas y por qué son necesarias.
-   Aprender a definir una ruta con un parámetro de URL (ej: `/link/:id`).
-   Usar el Hook `useParams` para acceder al valor del parámetro en un componente.
-   Crear una página de detalle para un enlace individual.

## Tareas

### 1. ¿Qué es una Ruta Dinámica?

Hasta ahora, nuestras rutas (`/` y `/admin`) son estáticas. Siempre apuntan al mismo componente.

Pero, ¿y si quisiéramos tener una página separada para cada uno de nuestros enlaces? Por ejemplo:
-   `/link/1` para ver los detalles del enlace con `id: 1`.
-   `/link/2` para ver los detalles del enlace con `id: 2`.

No vamos a crear una ruta en nuestro código para cada `id` posible. En su lugar, creamos una **ruta dinámica**. Definimos un patrón de ruta que incluye un **parámetro**, una parte de la URL que es variable.

### 2. Crear el Componente de Página de Detalle

Primero, creemos el componente que mostrará los detalles de un enlace.

**Paso a paso:**

1.  En la carpeta `src/pages`, crea un nuevo archivo llamado `LinkDetailPage.jsx`.
2.  Este componente necesitará obtener el `id` de la URL para saber qué enlace mostrar.

**Código inicial para `LinkDetailPage.jsx`:**

```jsx
import React from 'react';
import { useParams, Link } from 'react-router-dom';

function LinkDetailPage() {
  // Usamos el Hook useParams para obtener los parámetros de la ruta
  const { linkId } = useParams();

  // En una aplicación real, usarías este id para buscar los datos completos del enlace
  // (ya sea de una lista en el estado del padre o haciendo una nueva llamada a la API)

  return (
    <div>
      <h1 className="mb-3">Detalle del Enlace</h1>
      <p>Mostrando detalles para el enlace con ID: <strong>{linkId}</strong></p>
      {/* Aquí mostraríamos el título, la URL, etc. */}
      <div className="mt-4">
        <Link to="/" className="btn btn-primary">Volver al Perfil</Link>
      </div>
    </div>
  );
}

export default LinkDetailPage;
```

**¿Por qué y qué hace?**

*   **`import { useParams } ...`**: Importamos el Hook `useParams` de React Router.
*   **`const { linkId } = useParams();`**: `useParams()` devuelve un objeto que contiene los parámetros de la URL actual como pares clave-valor. La clave (`linkId`) la definiremos en el siguiente paso en nuestra configuración de rutas. Esto nos da acceso directo al valor dinámico de la URL.

### 3. Definir la Ruta Dinámica en `main.jsx`

Ahora, le diremos a React Router que existe esta nueva ruta dinámica.

**Paso a paso:**

1.  Abre `src/main.jsx`.
2.  Importa el nuevo componente `LinkDetailPage`.
3.  Añade una nueva ruta al array `children` del layout principal.

**Código a modificar en `main.jsx`:**

```jsx
// ... otras importaciones
import AdminPage from './pages/AdminPage.jsx';
import LinkDetailPage from './pages/LinkDetailPage.jsx'; // 2. Importar

// ...
const router = createBrowserRouter([
  {
    path: '/',
    element: <App />,
    children: [
      // ... otras rutas children
      {
        path: 'admin',
        element: <AdminPage />,
      },
      // 3. Añadir la nueva ruta dinámica
      {
        path: 'link/:linkId', // La sintaxis :nombre lo convierte en un parámetro
        element: <LinkDetailPage />,
      },
    ],
  },
]);
// ...
```

**¿Por qué y qué hace?**

*   **`path: 'link/:linkId'`**: Esta es la sintaxis clave.
    *   `link/`: Es la parte estática de la ruta.
    *   `:linkId`: El prefijo de dos puntos `:` le dice a React Router que esta parte de la URL es un **parámetro dinámico**. El nombre que le des (`linkId`) será la clave en el objeto que devuelva `useParams`.
*   Esta única definición de ruta ahora coincidirá con `/link/1`, `/link/abc`, `/link/cualquier-cosa`, etc.

### 4. Enlazar a la Página de Detalle

Finalmente, necesitamos una forma de navegar a estas nuevas páginas. Modificaremos nuestro `LinkCard` para que el título del enlace nos lleve a su página de detalle.

**Paso a paso:**

1.  Abre `src/components/LinkCard.jsx`.
2.  Importa el componente `Link` de `react-router-dom`.
3.  Reemplaza la etiqueta `<a>` con el componente `<Link>`.

**Código a modificar en `LinkCard.jsx`:**

```jsx
import React from 'react';
// 2. Importar Link
import { Link } from 'react-router-dom';

function LinkCard({ id, title, url, onDelete }) {
  const handleDelete = () => {
    onDelete(id);
  };

  return (
    <div className="card card-body mb-3 position-relative">
      {/* 3. Reemplazar <a> con <Link> */}
      <Link to={`/link/${id}`} className="text-decoration-none">
        <h5 className="mb-0">{title}</h5>
      </Link>
      <small className="text-muted">{url}</small>
      
      <button onClick={handleDelete} className="btn btn-danger btn-sm position-absolute top-0 end-0 m-2"> 
        Eliminar
      </button>
    </div>
  );
}

export default LinkCard;
```

**¿Por qué y qué hace?**

*   **`<Link to={...}>`**: Reemplazamos la etiqueta `<a>` que abría en una nueva pestaña por el componente `Link` de React Router para la navegación interna.
*   **``to={`/link/${id}`}``**: Estamos usando una plantilla de string de JavaScript para construir dinámicamente la URL de destino. Para cada tarjeta, el `id` del enlace se insertará en la URL, creando enlaces como `/link/1`, `/link/2`, etc.

Ahora, cuando hagas clic en el título de cualquier enlace en tu página de perfil, serás llevado a la página de detalles de ese enlace sin recargar la página. La página de detalles leerá el `id` de la URL y te mostrará para qué enlace estás viendo los detalles. Has aprendido a crear una estructura de navegación mucho más compleja y útil.

# Día 11: Navegación con React Router

## Objetivo del Día

Convertir nuestra aplicación de una sola vista a una aplicación multi-página (Single Page Application - SPA) utilizando la librería `react-router-dom`. Crearemos una nueva ruta `/admin` donde vivirá el formulario para añadir enlaces.

## Concepto Teórico

Las SPAs parecen tener múltiples páginas, pero en realidad nunca recargan el HTML. Una librería de enrutamiento como React Router intercepta los clics en los enlaces y cambia la URL en el navegador, renderizando diferentes componentes de React en respuesta, todo en el lado del cliente.

**Componentes Clave de React Router:**

-   `<BrowserRouter>`: Envuelve toda tu aplicación. Conecta tu app al historial del navegador.
-   `<Routes>`: Contenedor para todas tus rutas individuales.
-   `<Route>`: Define una ruta individual. Tiene dos props principales:
    -   `path`: La URL de la ruta (ej: `"/"` o `"/admin"`).
    -   `element`: El componente que se debe renderizar cuando la URL coincide con el `path`.
-   `<Link>`: Se usa en lugar de `<a>` para crear enlaces de navegación interna sin recargar la página.

## Dibujo Conceptual

React Router actúa como un controlador de tráfico, mostrando diferentes "vistas" (componentes) según la URL.

```
      URL del Navegador: http://localhost:5173/admin
              |
              v
      +---------------------------------+
      |       <BrowserRouter>           |
      |                                 |
      |         <Routes>                |
      |                                 |
      | <Route path="/" element={<Home/>} /> |
      |                                 |
      | <Route path="/admin" element={<Admin/>} /> | <-- Coincide!
      |                                 |
      |         </Routes>               |
      |                                 |
      +---------------------------------+
              |
              v
      React renderiza el componente <Admin />
```

## Tarea

1.  Instalar `react-router-dom`.
2.  Configurar las rutas en `main.jsx` o `App.jsx`.
3.  Crear componentes "página" para la vista principal y la de administración.

### Paso a Paso

1.  **Instalar la Librería:**
    *   Detén tu servidor de desarrollo (Ctrl+C).
    *   En la terminal, dentro de la carpeta del proyecto, ejecuta:
        ```bash
        npm install react-router-dom
        ```
    *   Vuelve a iniciar el servidor: `npm run dev`.

2.  **Crear Páginas (Pages):**
    *   Crea una nueva carpeta `src/pages`.
    *   Crea `HomePage.jsx`: Este contendrá lo que antes estaba en `App.jsx` (el perfil y la lista de enlaces).
    *   Crea `AdminPage.jsx`: Este contendrá el formulario para añadir enlaces.

3.  **Configurar Rutas en `App.jsx`:**
    *   Vamos a convertir `App.jsx` en nuestro "layout" principal y controlador de rutas.

    ```jsx
    // src/App.jsx
    import { Routes, Route, Link } from 'react-router-dom';
    import { HomePage } from './pages/HomePage';
    import { AdminPage } from './pages/AdminPage';
    // ... otros imports y datos que ahora pasarán a las páginas

    function App() {
      // El estado y la lógica para manejar los links se quedan aquí,
      // ya que ambos componentes (HomePage y AdminPage) lo necesitan.
      const [profile, setProfile] = useState(initialProfileData);
      const [links, setLinks] = useState(initialLinks);

      const addLink = (newLinkData) => { ... };

      return (
        <div className="App">
          <nav>
            <Link to="/">Ver Perfil</Link> | <Link to="/admin">Admin</Link>
          </nav>
          <hr />
          <Routes>
            <Route path="/" element={
              <HomePage profile={profile} links={links} />
            } />
            <Route path="/admin" element={
              <AdminPage onNewLink={addLink} />
            } />
          </Routes>
        </div>
      );
    }
    ```
    *   **Importante:** No olvides envolver tu `<App />` en `<BrowserRouter>` dentro de `src/main.jsx`:

    ```jsx
    // src/main.jsx
    import React from 'react';
    import ReactDOM from 'react-dom/client';
    import { BrowserRouter } from 'react-router-dom';
    import App from './App';
    import './index.css';

    ReactDOM.createRoot(document.getElementById('root')).render(
      <React.StrictMode>
        <BrowserRouter>
          <App />
        </BrowserRouter>
      </React.StrictMode>
    );
    ```

4.  **Crear el contenido de las páginas `HomePage.jsx` y `AdminPage.jsx`** moviendo el JSX correspondiente desde el `App.jsx` antiguo.

### Verifica el Resultado

Ahora tienes una barra de navegación. Al hacer clic en "Admin", la URL cambiará a `/admin` y verás el formulario. Al hacer clic en "Ver Perfil", volverás a la página principal. ¡Todo sin recargar la página! Has creado una verdadera SPA.

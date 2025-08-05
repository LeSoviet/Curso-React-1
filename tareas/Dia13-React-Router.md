# Día 13: Introducción a React Router

## Objetivo del Día

Hasta ahora, nuestra aplicación es una "Single Page Application" (SPA) en el sentido más literal: solo tiene una página. Hoy aprenderás a crear una navegación multi-página utilizando `react-router-dom`, la librería estándar para el enrutamiento en React.

## Tarea

Dividirás la aplicación en dos rutas: la página de perfil pública (`/`) y una página de administración (`/admin`) donde se podrán añadir y eliminar enlaces.

### Paso a Paso

1.  **Instalar React Router DOM:**
    *   Detén el servidor de desarrollo.
    *   En la terminal, dentro de `linkhub-project`, ejecuta:

    ```bash
    npm install react-router-dom
    ```

2.  **Configurar el Router en `main.jsx`:**
    *   Abre `src/main.jsx`. Aquí es donde envolveremos toda la aplicación en el contexto del router.
    *   Importa `BrowserRouter` de `react-router-dom`.
    *   Envuelve tu componente `<App />` con `<BrowserRouter>`.

    ```jsx
    // src/main.jsx
    import React from 'react';
    import ReactDOM from 'react-dom/client';
    import App from './App.jsx';
    import { BrowserRouter } from 'react-router-dom';
    import 'bootstrap/dist/css/bootstrap.min.css';

    ReactDOM.createRoot(document.getElementById('root')).render(
      <React.StrictMode>
        <BrowserRouter>
          <App />
        </BrowserRouter>
      </React.StrictMode>
    );
    ```

3.  **Definir las Rutas en `App.jsx`:**
    *   Abre `src/App.jsx`. Importa `Routes` y `Route` de `react-router-dom`.
    *   Dentro del `return` de `App`, usarás el componente `<Routes>` para definir las diferentes rutas de tu aplicación.
    *   Cada `<Route>` necesita dos props: `path` (la URL) y `element` (el componente a renderizar para esa ruta).
    *   Por ahora, podemos usar el mismo contenido para ambas rutas, pero lo separaremos en el siguiente paso.

    ```jsx
    // src/App.jsx
    import { Routes, Route } from 'react-router-dom';
    // ...otras importaciones

    function App() {
      // ...toda tu lógica de estado actual

      return (
        <div className="container text-center mt-5">
          <ProfileHeader /* ...props... */ />

          <Routes>
            <Route path="/" element={ <LinkList links={links} /> } />
            <Route path="/admin" element={ 
              <>
                <AddLinkForm onNewLink={addLink} />
                <LinkList links={links} onDeleteLink={handleDeleteLink} />
              </>
            } />
          </Routes>
        </div>
      );
    }
    ```
    *   Nota que para `/admin`, hemos envuelto los dos componentes en un Fragmento (`<>...</>`) porque `element` solo puede recibir un único elemento.
    *   En la ruta `/`, solo mostramos la lista, sin el botón de eliminar. Para ello, no le pasamos la prop `onDeleteLink` a `LinkList`.

4.  **Añadir Navegación (`Link`):**
    *   Para poder cambiar entre rutas sin recargar la página, usamos el componente `Link` de React Router.
    *   Añade un par de enlaces de navegación en `App.jsx`, fuera del `<Routes>`.

    ```jsx
    // src/App.jsx
    // ...en el return, justo debajo de ProfileHeader

    <nav className="nav nav-pills justify-content-center my-4">
      <Link to="/" className="nav-link">Ver Perfil</Link>
      <Link to="/admin" className="nav-link">Administrar</Link>
    </nav>

    <Routes>
      {/* ... */}
    </Routes>
    ```
    *   No olvides importar `Link` de `react-router-dom`.

5.  **Probar las Rutas:**
    *   Inicia el servidor (`npm run dev`).
    *   Verás los enlaces de navegación. Haz clic en ellos. La URL en el navegador cambiará entre `/` y `/admin`, y el contenido se actualizará sin recargar la página.
    *   En la ruta `/admin`, deberías ver el formulario y la lista con botones de eliminar. En la ruta `/`, solo deberías ver la lista de enlaces, sin el formulario ni los botones de eliminar.

Has convertido tu SPA en una aplicación con múltiples vistas, abriendo la puerta a construir aplicaciones mucho más complejas y organizadas.
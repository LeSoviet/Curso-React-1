# Día 14: Creando Páginas como Componentes

## Objetivo del Día

Nuestro archivo `App.jsx` está empezando a llenarse de lógica que pertenece a diferentes páginas. Hoy organizarás mejor tu código creando componentes específicos para cada página o ruta, un patrón muy común en aplicaciones con enrutamiento.

## Tarea

Crearás dos nuevos componentes-página: `PublicProfilePage` y `AdminPage`. Moverás la lógica y el JSX correspondiente de `App.jsx` a estos nuevos archivos, dejando `App.jsx` como el principal organizador de rutas y estado global.

### Paso a Paso

1.  **Crear la Carpeta `pages`:**
    *   Para mantener el proyecto ordenado, crea una nueva carpeta `src/pages`.

2.  **Crear el Componente `PublicProfilePage.jsx`:**
    *   Dentro de `src/pages`, crea el archivo `PublicProfilePage.jsx`.
    *   Este componente representará la vista pública (`/`). Su única responsabilidad es mostrar la lista de enlaces.
    *   Recibirá la lista de `links` como prop.

    ```jsx
    // src/pages/PublicProfilePage.jsx
    import LinkList from '../components/LinkList';

    function PublicProfilePage({ links }) {
      return <LinkList links={links} />;
    }

    export default PublicProfilePage;
    ```

3.  **Crear el Componente `AdminPage.jsx`:**
    *   Dentro de `src/pages`, crea el archivo `AdminPage.jsx`.
    *   Este componente representará la vista de administración (`/admin`).
    *   Recibirá como props la lista de `links`, la función para añadir un enlace (`onNewLink`) y la función para eliminar un enlace (`onDeleteLink`).

    ```jsx
    // src/pages/AdminPage.jsx
    import AddLinkForm from '../components/AddLinkForm';
    import LinkList from '../components/LinkList';

    function AdminPage({ links, onNewLink, onDeleteLink }) {
      return (
        <>
          <AddLinkForm onNewLink={onNewLink} />
          <hr />
          <LinkList links={links} onDeleteLink={onDeleteLink} />
        </>
      );
    }

    export default AdminPage;
    ```

4.  **Simplificar `App.jsx`:**
    *   Ahora `App.jsx` se vuelve mucho más limpio. Su rol es manejar el estado y definir las rutas, pero delega el renderizado de cada página a los componentes que acabas de crear.
    *   Importa `PublicProfilePage` y `AdminPage`.
    *   Actualiza el componente `<Routes>` para usar tus nuevos componentes-página en la prop `element`.

    ```jsx
    // src/App.jsx
    import { useState, useEffect } from 'react';
    import { Routes, Route, Link } from 'react-router-dom';

    // Componentes
    import ProfileHeader from './components/ProfileHeader';

    // Páginas
    import PublicProfilePage from './pages/PublicProfilePage';
    import AdminPage from './pages/AdminPage';

    function App() {
      // ...toda tu lógica de estado (useState, useEffect, addLink, handleDeleteLink) sigue aquí...

      return (
        <div className="container text-center mt-5">
          <ProfileHeader /* ...props... */ />
          <nav /* ... */ >
            {/* ...Links de navegación... */}
          </nav>

          <Routes>
            <Route path="/" element={<PublicProfilePage links={links} />} />
            <Route 
              path="/admin" 
              element={<AdminPage links={links} onNewLink={addLink} onDeleteLink={handleDeleteLink} />}
            />
          </Routes>
        </div>
      );
    }

    export default App;
    ```

5.  **Verificar la Aplicación:**
    *   La aplicación debe funcionar exactamente igual que antes.
    *   Sin embargo, la estructura de tu proyecto es ahora mucho más escalable. Si quisieras añadir una página de "Estadísticas", simplemente crearías `StatisticsPage.jsx` y añadirías una nueva `<Route>` en `App.jsx`, sin complicar los componentes existentes.

Has dado un paso importante para estructurar proyectos de React más grandes. Separar las preocupaciones en componentes, y luego en páginas, es clave para mantener la cordura a medida que la aplicación crece.
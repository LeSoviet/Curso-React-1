# Día 15: Compartiendo Estado Global con `useContext`

## Objetivo del Día

Hoy abordarás un problema común en React: **"prop drilling"** (perforación de props). Esto sucede cuando tienes que pasar props a través de varios niveles de componentes que en realidad no las necesitan, solo para hacerlas llegar a un componente anidado que sí las usa.

El hook `useContext` te permite crear un "estado global" para una parte de tu árbol de componentes, haciendo que ese estado esté disponible para cualquier componente dentro de ese árbol sin necesidad de pasarlo manualmente por cada nivel.

## Tarea

Crearás un `LinksContext` para gestionar todo lo relacionado con los enlaces (la lista, añadir, eliminar). Los componentes `AdminPage` y `PublicProfilePage` consumirán este contexto directamente, eliminando la necesidad de que `App.jsx` les pase las props.

### Paso a Paso

1.  **Crear el Archivo de Contexto:**
    *   Crea una nueva carpeta `src/context`.
    *   Dentro, crea un archivo `LinksContext.jsx`.
    *   Importa `createContext` de React. Llama a esta función para crear tu contexto.

    ```jsx
    // src/context/LinksContext.jsx
    import { createContext } from 'react';

    export const LinksContext = createContext();
    ```

2.  **Crear el Componente "Proveedor" (Provider):**
    *   En el mismo archivo, `LinksContext.jsx`, crea un componente que actuará como proveedor. Este componente contendrá toda la lógica de estado que actualmente vive en `App.jsx`.
    *   Este componente `LinksProvider` renderizará `LinksContext.Provider` y pasará el estado y las funciones a través de la prop `value`.

    ```jsx
    // src/context/LinksContext.jsx
    import { createContext, useState, useEffect } from 'react';

    export const LinksContext = createContext();

    export function LinksProvider({ children }) {
      const [links, setLinks] = useState(() => { /* ...lógica de localStorage... */ });

      useEffect(() => { /* ...lógica de guardar en localStorage... */ }, [links]);

      const addLink = (linkData) => { /* ...lógica de añadir... */ };
      const handleDeleteLink = (idToDelete) => { /* ...lógica de eliminar... */ };

      const value = { links, addLink, handleDeleteLink };

      return <LinksContext.Provider value={value}>{children}</LinksContext.Provider>;
    }
    ```

3.  **Envolver la Aplicación con el Provider:**
    *   En `App.jsx`, importa `LinksProvider`.
    *   Mueve toda la lógica de estado de los enlaces (`useState`, `useEffect`, `addLink`, `handleDeleteLink`) de `App.jsx` a `LinksProvider` (ya lo hiciste en el paso anterior, ahora bórrala de `App`).
    *   Envuelve tu `<Routes>` con el `<LinksProvider>`.

    ```jsx
    // src/App.jsx
    import { LinksProvider } from './context/LinksContext';
    // ...

    function App() {
      // El estado de los enlaces ya no vive aquí

      return (
        <LinksProvider>
          <div className="container text-center mt-5">
            {/* ...ProfileHeader y Nav... */}
            <Routes>
              <Route path="/" element={<PublicProfilePage />} />
              <Route path="/admin" element={<AdminPage />} />
            </Routes>
          </div>
        </LinksProvider>
      );
    }
    ```

4.  **Consumir el Contexto en las Páginas:**
    *   Importa `useContext` y `LinksContext` en `AdminPage.jsx` y `PublicProfilePage.jsx`.
    *   Llama a `useContext(LinksContext)` para obtener acceso directo al `value` del proveedor.
    *   Ya no necesitas recibir las props `links`, `onNewLink`, etc.

    ```jsx
    // src/pages/AdminPage.jsx
    import { useContext } from 'react';
    import { LinksContext } from '../context/LinksContext';
    // ...

    function AdminPage() {
      const { links, addLink, handleDeleteLink } = useContext(LinksContext);
      return (
        <>
          <AddLinkForm onNewLink={addLink} />
          <hr />
          <LinkList links={links} onDeleteLink={handleDeleteLink} />
        </>
      );
    }
    ```

    ```jsx
    // src/pages/PublicProfilePage.jsx
    import { useContext } from 'react';
    import { LinksContext } from '../context/LinksContext';
    import LinkList from '../components/LinkList';

    function PublicProfilePage() {
      const { links } = useContext(LinksContext);
      return <LinkList links={links} />;
    }
    ```

5.  **Verificar:**
    *   La aplicación debe funcionar perfectamente, como antes.
    *   La gran diferencia es la limpieza del código. `App.jsx` ya no está sobrecargado. Los componentes-página obtienen los datos que necesitan directamente del contexto, sin que `App` tenga que actuar como intermediario.

¡Felicidades por completar los 15 días! Has aprendido a manejar un problema de arquitectura de software común y a usar `useContext` para crear un estado global y desacoplar tus componentes. Este es un gran paso hacia la construcción de aplicaciones React más grandes y mantenibles.
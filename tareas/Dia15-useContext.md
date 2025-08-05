# Día 15: Evitando "Prop Drilling" con `useContext`

## Objetivos del Día

-   Entender el problema conocido como "prop drilling" (perforación de props).
-   Aprender a crear un "Context" de React para proporcionar datos a un árbol de componentes.
-   Usar el Hook `useContext` para consumir datos de un Contexto sin pasarlos a través de props.
-   Refactorizar la lógica de manejo de enlaces para usar Context.

## Tareas

### 1. El Problema: "Prop Drilling"

Imagina que tienes un componente `App` que tiene información sobre el usuario autenticado. Ahora, un componente `Avatar` muy anidado en el árbol de componentes necesita mostrar la imagen de ese usuario.

```
App (tiene `user`)
  Layout
    Header
      Navbar
        UserMenu
          Avatar (necesita `user.imageUrl`)
```

Para que `Avatar` obtenga la información, tendrías que pasar la `prop` `user` a través de cada componente intermedio (`Layout`, `Header`, `Navbar`, `UserMenu`), aunque esos componentes no la necesiten para nada. Esto se llama **"prop drilling"**. 

Es un problema porque:
-   **Es tedioso:** Tienes que añadir la `prop` en cada nivel.
-   **Acopla los componentes:** Los componentes intermedios ahora dependen de una `prop` que no usan, lo que los hace menos reutilizables.
-   **Dificulta la refactorización:** Si cambias la forma de los datos, tienes que actualizarla en toda la cadena.

### 2. La Solución: React Context

React Context nos permite crear una especie de "tubería" de datos global para un árbol de componentes. 

El sistema tiene dos partes:
1.  **El Proveedor (`Provider`):** Es un componente que "provee" o emite un valor. Envuelves un árbol de componentes con este `Provider` y todos los componentes dentro de ese árbol (sin importar cuán anidados estén) podrán acceder al valor.
2.  **El Consumidor (`useContext`):** Es un Hook que permite a cualquier componente funcional dentro del `Provider` "suscribirse" y leer el valor proporcionado.

### 3. Creando un `LinksContext`

Vamos a refactorizar nuestro `LinkManager` para que, en lugar de renderizar la UI directamente, actúe como un `Provider` que ofrece los `links`, `loading`, `error` y las funciones para manipularlos.

**Paso a paso:**

1.  En `src/components`, renombra `LinkManager.jsx` a `LinksProvider.jsx` para reflejar mejor su nuevo propósito.
2.  Crea y exporta un Contexto de React.
3.  Modifica el componente para que devuelva un `Provider` en lugar de JSX de UI.

**Código para `src/components/LinksProvider.jsx`:**

```jsx
import React, { useState, useEffect, createContext } from 'react';
import { getLinks } from '../services/linkService';

// 2. Crear el Contexto
export const LinksContext = createContext();

// El componente ahora se llama LinksProvider
export function LinksProvider({ children }) {
  const [links, setLinks] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // ... (toda la lógica de useEffects y los handlers handleAddLink/handleDeleteLink permanece EXACTAMENTE IGUAL)
  const handleAddLink = (title, url) => { /*...*/ };
  const handleDeleteLink = (id) => { /*...*/ };

  // El valor que será accesible para todos los consumidores del contexto
  const value = {
    links,
    loading,
    error,
    addLink: handleAddLink,
    deleteLink: handleDeleteLink,
  };

  // 3. Devolver el Provider envolviendo a los hijos
  return (
    <LinksContext.Provider value={value}>
      {children} 
    </LinksContext.Provider>
  );
}
```

**¿Por qué y qué hace?**

*   **`createContext()`**: Esta función de React crea un objeto de contexto. Lo exportamos para poder usarlo en otros componentes.
*   **`export function LinksProvider({ children })`**: Nuestro componente ahora acepta una `prop` especial, `children`. `children` representa cualquier componente o JSX que esté anidado dentro de nuestro `Provider`.
*   **`const value = { ... }`**: Creamos un objeto que contiene todos los datos y funciones que queremos hacer disponibles globalmente.
*   **`<LinksContext.Provider value={value}>`**: Este es el componente `Provider`. Le pasamos nuestro objeto `value` a su `prop` `value`.
*   **`{children}`**: Al renderizar `children` dentro del `Provider`, estamos diciendo: "Cualquier componente que se renderice aquí tendrá acceso al `value` de este contexto".

### 4. Envolviendo la Aplicación con el `Provider`

Para que el contexto esté disponible, tenemos que envolver nuestra aplicación (o la parte de ella que lo necesite) con el `Provider`.

**Paso a paso:**

1.  Abre `src/pages/ProfilePage.jsx`.
2.  Envuelve el contenido de la página con `LinksProvider`.

**Código a modificar en `ProfilePage.jsx`:**

```jsx
import React from 'react';
import ProfileHeader from '../components/ProfileHeader';
import { LinksProvider } from '../components/LinksProvider'; // Importar el Provider
import LinksDashboard from '../components/LinksDashboard'; // Un nuevo componente que crearemos

function ProfilePage() {
  const userProfile = { /*...*/ };

  return (
    // Envolvemos todo con el Provider
    <LinksProvider>
      <ProfileHeader {...} />
      <hr />
      {/* Este componente ahora estará dentro del contexto */}
      <LinksDashboard />
    </LinksProvider>
  );
}

export default ProfilePage;
```

### 5. Consumiendo el Contexto con `useContext`

Ahora, cualquier componente dentro de `LinksProvider` puede acceder a los datos. Crearemos un `LinksDashboard` que consuma el contexto y renderice la UI.

**Paso a paso:**

1.  Crea un nuevo archivo `src/components/LinksDashboard.jsx`.
2.  Usa el Hook `useContext` para acceder a los datos y funciones del `LinksContext`.

**Código para `LinksDashboard.jsx`:**

```jsx
import React, { useContext } from 'react';
import { LinksContext } from './LinksProvider'; // Importar el Contexto
import AddLinkForm from './AddLinkForm';
import LinkList from './LinkList';

function LinksDashboard() {
  // 2. Usar el Hook useContext para obtener el valor del Provider
  const { links, loading, error, addLink, deleteLink } = useContext(LinksContext);

  const renderContent = () => {
    if (loading) return <p>Cargando...</p>;
    if (error) return <p>Error...</p>;
    if (links.length === 0) return <p>No hay enlaces.</p>;
    return <LinkList links={links} onDelete={deleteLink} />;
  };

  return (
    <div>
      <AddLinkForm onAddLink={addLink} />
      <div className="mt-4">{renderContent()}</div>
    </div>
  );
}

export default LinksDashboard;
```

**¿Por qué y qué hace?**

*   **`useContext(LinksContext)`**: Este Hook busca en el árbol de componentes el `Provider` de `LinksContext` más cercano y devuelve el `value` que ese `Provider` está ofreciendo.
*   **¡Sin Prop Drilling!**: Observa que `LinksDashboard` no recibe ninguna `prop`. Obtiene todo lo que necesita directamente del contexto. `ProfilePage` ya no necesita saber nada sobre `handleAddLink` o `handleDeleteLink`.

Has aprendido a usar uno de los Hooks más potentes de React para manejar el estado global de una forma limpia y desacoplada. Esto es especialmente útil para datos que son necesarios en muchas partes de la aplicación, como información del usuario, el tema (claro/oscuro) de la UI, o el idioma seleccionado.

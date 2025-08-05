# Día 14: Componiendo Páginas a partir de Componentes

## Objetivos del Día

-   Refactorizar componentes grandes (páginas) en piezas más pequeñas y manejables.
-   Entender cómo "componer" una página ensamblando varios componentes más pequeños.
-   Mejorar la legibilidad y la mantenibilidad de nuestra `ProfilePage`.

## Tareas

### 1. El Problema: Componentes que Crecen Demasiado

Nuestro componente `ProfilePage.jsx` ha empezado a crecer. Actualmente, es responsable de:

1.  Manejar el estado de `links`, `loading` y `error`.
2.  Contener la lógica para cargar, añadir y eliminar enlaces (`useEffect`, `handleAddLink`, `handleDeleteLink`).
3.  Renderizar el `ProfileHeader`, el `AddLinkForm` y el `LinkList`.

Aunque hemos hecho un buen trabajo separando los componentes de presentación, la propia página sigue teniendo muchas responsabilidades. A medida que una aplicación crece, estos componentes de página pueden volverse muy grandes y difíciles de entender.

La solución es la misma que hemos aplicado antes, pero a un nivel más micro: **dividir y conquistar**. Podemos pensar en una página no como un bloque monolítico, sino como un ensamblaje de componentes más pequeños y enfocados.

### 2. Aislando la Lógica de los Enlaces

La mayor parte de la lógica en `ProfilePage` está relacionada con la gestión de los enlaces. ¿Qué pasaría si extraemos todo eso a su propio componente?

Vamos a crear un componente "contenedor" que se encargue exclusivamente de la lógica de los enlaces: `LinkManager`.

**Paso a paso:**

1.  En la carpeta `src/components`, crea un nuevo archivo llamado `LinkManager.jsx`.
2.  **Mueve toda la lógica de estado y los manejadores de eventos** relacionados con los enlaces desde `ProfilePage.jsx` a `LinkManager.jsx`.

**Código para `LinkManager.jsx`:**

```jsx
import React, { useState, useEffect } from 'react';
import { getLinks } from '../services/linkService';
import AddLinkForm from './AddLinkForm';
import LinkList from './LinkList';

function LinkManager() {
  // 1. Toda la lógica de estado de los enlaces vive aquí ahora
  const [links, setLinks] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // 2. El useEffect para cargar datos vive aquí
  useEffect(() => {
    const savedLinks = localStorage.getItem('linkhub_links');
    if (savedLinks) {
      setLinks(JSON.parse(savedLinks));
      setLoading(false);
    } else {
      getLinks()
        .then(apiLinks => {
          const adaptedLinks = apiLinks.map(link => ({ id: link.id, title: link.title, url: `https://jsonplaceholder.typicode.com/todos/${link.id}` }));
          setLinks(adaptedLinks);
        })
        .catch(err => setError(err))
        .finally(() => setLoading(false));
    }
  }, []);

  // 3. El useEffect para guardar datos vive aquí
  useEffect(() => {
    if (!loading) {
      localStorage.setItem('linkhub_links', JSON.stringify(links));
    }
  }, [links, loading]);

  // 4. Los manejadores de eventos viven aquí
  const handleAddLink = (title, url) => {
    const newLink = { id: Date.now(), title, url };
    setLinks(prevLinks => [...prevLinks, newLink]);
  };

  const handleDeleteLink = (id) => {
    setLinks(prevLinks => prevLinks.filter(link => link.id !== id));
  };

  // 5. Este componente ahora renderiza el formulario y la lista
  return (
    <div>
      <AddLinkForm onAddLink={handleAddLink} />
      
      {loading && <p className="text-center">Cargando enlaces...</p>}
      {error && <p className="text-center text-danger">Error al cargar los enlaces.</p>}
      {!loading && !error && <LinkList links={links} onDelete={handleDeleteLink} />}
    </div>
  );
}

export default LinkManager;
```

**¿Por qué y qué hace?**

*   Hemos creado un componente contenedor (`LinkManager`) que está **altamente enfocado**. Su única responsabilidad es gestionar todo lo relacionado con los enlaces.
*   No se preocupa por el perfil del usuario, la navegación o el layout de la página. Solo se ocupa de la lógica de los enlaces.
*   **`setLinks(prevLinks => ...)`**: Hemos cambiado a la forma funcional de actualizar el estado. En lugar de `setLinks([...links, newLink])`, usamos `setLinks(prevLinks => [...prevLinks, newLink])`. `prevLinks` es el valor más reciente del estado. Esta forma es más segura cuando el nuevo estado depende del anterior.

### 3. Simplificando `ProfilePage.jsx`

Ahora que `LinkManager` hace todo el trabajo pesado, nuestra `ProfilePage` se vuelve increíblemente simple y declarativa. Se convierte en un verdadero componente de "página" que ensambla las diferentes secciones.

**Paso a paso:**

1.  Abre `src/pages/ProfilePage.jsx`.
2.  Elimina toda la lógica de estado y los manejadores de eventos que moviste.
3.  Importa y utiliza el nuevo componente `LinkManager`.

**Código final para `ProfilePage.jsx`:**

```jsx
import React from 'react';
import ProfileHeader from '../components/ProfileHeader';
import LinkManager from '../components/LinkManager'; // 3. Importar

function ProfilePage() {
  // ¡Mira qué limpio está esto!
  const userProfile = {
    name: 'John Doe',
    bio: 'Software Developer | React Enthusiast',
    imageUrl: 'https://via.placeholder.com/150'
  };

  return (
    <div>
      <ProfileHeader 
        name={userProfile.name}
        bio={userProfile.bio}
        imageUrl={userProfile.imageUrl}
      />
      <hr />
      {/* Simplemente renderizamos el gestor de enlaces */}
      <LinkManager />
    </div>
  );
}

export default ProfilePage;
```

**¿Por qué y qué hace?**

*   `ProfilePage` ahora es extremadamente fácil de leer. Su trabajo es claro: mostrar un encabezado de perfil y el gestor de enlaces.
*   Hemos logrado una excelente **separación de preocupaciones**. Si hay un bug con la carga de enlaces, sabes que el problema está en `LinkManager`, no en `ProfilePage`. Si quieres cambiar cómo se ve el perfil, modificas `ProfileHeader`.
*   Esta técnica de **composición** (construir UIs complejas anidando y combinando componentes) es el superpoder de React. Te permite manejar la complejidad dividiendo los problemas en piezas más pequeñas y reutilizables.

Al refactorizar de esta manera, has hecho tu base de código mucho más mantenible. Es más fácil para ti (y para otros desarrolladores) razonar sobre ella, depurarla y añadir nuevas funcionalidades en el futuro.

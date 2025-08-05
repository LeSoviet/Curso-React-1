# Día 9: Refactorizando con Componentes de Presentación

## Objetivos del Día

-   Continuar aplicando el patrón Contenedor/Presentación.
-   Crear un componente de presentación para el encabezado del perfil (`ProfileHeader`).
-   Hacer que `App.jsx` sea aún más limpio y declarativo.

## Tareas

### 1. Revisando `App.jsx`

Nuestro `App.jsx` ha mejorado mucho. Ahora actúa como un contenedor que maneja el estado y la lógica. Sin embargo, todavía tiene algo de JSX de presentación directamente en su `return`:

```jsx
// En App.jsx
return (
  <div className="container mt-5">
    {/* Esto es JSX de presentación */}
    <section className="profile-header text-center mb-4">
      <img 
        src="https://via.placeholder.com/150"
        alt="Profile"
        className="rounded-circle mb-3"
      />
      <h1>John Doe</h1>
      <p className="text-muted">Software Developer | React Enthusiast</p>
    </section>

    <AddLinkForm onAddLink={handleAddLink} />
    <LinkList links={links} onDelete={handleDeleteLink} />
  </div>
);
```

El objetivo de un componente contenedor puro es delegar todo el renderizado a los componentes de presentación. Vamos a extraer esa sección del perfil a su propio componente.

### 2. Crear el componente `ProfileHeader.jsx`

Este será un componente de presentación muy simple. Su única responsabilidad será mostrar la información del perfil que reciba por `props`.

**Paso a paso:**

1.  En la carpeta `src/components`, crea un nuevo archivo llamado `ProfileHeader.jsx`.
2.  Define el componente para que acepte `props` como `imageUrl`, `name`, y `bio`.
3.  Copia el JSX de la sección del perfil desde `App.jsx` y pégalo en el `return` de `ProfileHeader.jsx`, reemplazando el contenido estático con las `props`.

**Código para `ProfileHeader.jsx`:**

```jsx
import React from 'react';

// Recibe los datos del perfil a través de props
function ProfileHeader({ imageUrl, name, bio }) {
  return (
    <section className="profile-header text-center mb-4">
      <img 
        src={imageUrl} // Usa la prop imageUrl
        alt={`Perfil de ${name}`} // Usa la prop name para un alt text dinámico
        className="img-fluid rounded-circle mb-3" // img-fluid hace la imagen responsiva
        style={{ width: '150px', height: '150px' }} // Estilo en línea para tamaño fijo
      />
      <h1>{name}</h1> {/* Usa la prop name */}
      <p className="text-muted">{bio}</p> {/* Usa la prop bio */}
    </section>
  );
}

export default ProfileHeader;
```

**¿Por qué y qué hace?**

*   Hemos creado un componente completamente reutilizable. Ahora podríamos tener diferentes perfiles en nuestra aplicación simplemente pasándole diferentes `props` a `ProfileHeader`.
*   **`alt={\`Perfil de ${name}\`}`**: Estamos usando una plantilla de string de JavaScript para crear un texto `alt` más descriptivo, lo cual es excelente para la accesibilidad.
*   **`className="img-fluid ..."`**: `img-fluid` es una clase de Bootstrap que asegura que la imagen nunca sea más ancha que su contenedor, haciéndola responsiva.
*   **`style={{...}}`**: Además de `className`, puedes pasar estilos directamente a un elemento con el atributo `style`. Espera un objeto de JavaScript donde las propiedades de CSS se escriben en `camelCase` (ej: `backgroundColor` en lugar de `background-color`). Lo usamos aquí para asegurar que la imagen de perfil mantenga una forma cuadrada.

### 3. Usar `ProfileHeader` en `App.jsx`

Ahora, vamos a limpiar `App.jsx` reemplazando el bloque de JSX que cortamos con nuestro nuevo componente.

**Paso a paso:**

1.  Abre `src/App.jsx`.
2.  Importa el nuevo componente `ProfileHeader`.
3.  En el `return`, elimina la sección del perfil y en su lugar, renderiza el componente `<ProfileHeader />`, pasándole las `props` necesarias.

**Código a modificar en `App.jsx`:**

```jsx
// ... otras importaciones
import AddLinkForm from './components/AddLinkForm';
import LinkList from './components/LinkList';
import ProfileHeader from './components/ProfileHeader'; // 1. Importar

function App() {
  const [links, setLinks] = useState([/*...*/]);
  // ... lógica y useEffect ...

  // Datos del perfil (podrían venir del estado o de una API en el futuro)
  const userProfile = {
    name: 'John Doe',
    bio: 'Software Developer | React Enthusiast',
    imageUrl: 'https://via.placeholder.com/150'
  };

  return (
    <div className="container mt-5">
      {/* 2. Usar el nuevo componente */}
      <ProfileHeader 
        name={userProfile.name}
        bio={userProfile.bio}
        imageUrl={userProfile.imageUrl}
      />

      <AddLinkForm onAddLink={handleAddLink} />
      <LinkList links={links} onDelete={handleDeleteLink} />
    </div>
  );
}

export default App;
```

**¿Por qué y qué hace?**

*   Hemos creado un objeto `userProfile` para mantener los datos del perfil organizados. En una aplicación real, este objeto probablemente vendría del estado, que a su vez se llenaría con datos de una API.
*   El `return` de `App.jsx` es ahora extremadamente declarativo y fácil de leer. Describe la estructura de la página (`ProfileHeader`, `AddLinkForm`, `LinkList`) sin preocuparse por los detalles de implementación de cada sección.
*   Hemos reforzado la separación de responsabilidades. `App.jsx` se encarga de la **orquestación** (qué componentes mostrar y qué datos y funciones pasarles), mientras que los componentes en la carpeta `components` se encargan de la **presentación** (cómo se ven esos datos).

Este proceso de refactorización es un ciclo continuo en el desarrollo con React. A medida que los componentes crecen, busca oportunidades para extraer JSX en componentes de presentación más pequeños y especializados. Esto mantendrá tu base de código saludable y fácil de manejar a largo plazo.

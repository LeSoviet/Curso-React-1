# Día 8: Centralizando el Estado y la Lógica

## Objetivos del Día

-   Refactorizar `App.jsx` para que actúe como un "componente contenedor".
-   Crear un nuevo componente de "presentación" para la lista de enlaces.
-   Entender la diferencia entre componentes contenedores y de presentación.

## Tareas

### 1. Contenedores vs. Componentes de Presentación

A medida que tu aplicación crece, es una buena práctica separar las preocupaciones. Un patrón común en React es dividir los componentes en dos categorías:

1.  **Componentes Contenedores (o Inteligentes):**
    *   **¿Qué hacen?** Se preocupan por **cómo funcionan las cosas**.
    *   **Responsabilidades:** Manejan el estado, realizan llamadas a APIs, contienen la lógica de negocio (como `handleAddLink` o `handleDeleteLink`).
    *   **Ejemplo:** Nuestro `App.jsx` se está convirtiendo en un componente contenedor. Sabe de dónde vienen los datos de los enlaces y cómo modificarlos.

2.  **Componentes de Presentación (o Tontos):**
    *   **¿Qué hacen?** Se preocupan por **cómo se ven las cosas**.
    *   **Responsabilidades:** Reciben datos y funciones a través de `props` y los renderizan. No tienen su propio estado complejo (quizás algo de estado de UI, como si un menú está abierto o no).
    *   **Ejemplo:** `LinkCard.jsx` y `AddLinkForm.jsx` son componentes de presentación. No saben de dónde vienen los datos o qué sucede cuando se hace clic en un botón; simplemente reciben `props` y las usan.

Vamos a aplicar este patrón creando un componente de presentación para nuestra lista de enlaces.

### 2. Crear el componente `LinkList.jsx`

Este componente se encargará únicamente de mostrar la lista de `LinkCard`.

**Paso a paso:**

1.  En la carpeta `src/components`, crea un nuevo archivo llamado `LinkList.jsx`.
2.  Este componente recibirá la lista de `links` y la función `onDelete` como `props` y se encargará de hacer el `.map()`.

**Código para `LinkList.jsx`:**

```jsx
import React from 'react';
import LinkCard from './LinkCard';

// Recibe la lista de enlaces y la función de eliminación como props
function LinkList({ links, onDelete }) {
  return (
    <section className="links-container">
      {links.map(link => (
        <LinkCard
          key={link.id}
          id={link.id}
          title={link.title}
          url={link.url}
          onDelete={onDelete} // Pasa la función onDelete a cada LinkCard
        />
      ))}
    </section>
  );
}

export default LinkList;
```

**¿Por qué y qué hace?**

*   Hemos movido la lógica de renderizado de la lista (`.map()`) fuera de `App.jsx` y la hemos encapsulado en su propio componente.
*   `LinkList` es un componente de presentación puro. No sabe cómo se obtienen o eliminan los enlaces; su única responsabilidad es mostrarlos. Recibe la lista `links` y la función `onDelete` y las pasa hacia abajo a los `LinkCard`.

### 3. Refactorizar `App.jsx` para usar `LinkList`

Ahora que tenemos nuestro nuevo componente, podemos simplificar `App.jsx`, haciendo que su rol como "contenedor" sea aún más claro.

**Paso a paso:**

1.  Abre `src/App.jsx`.
2.  Importa el nuevo componente `LinkList`.
3.  Reemplaza la sección `<section className="links-container">` y el `.map()` con el uso de tu nuevo componente.

**Código a modificar en `App.jsx`:**

```jsx
// ... otras importaciones
import AddLinkForm from './components/AddLinkForm';
import LinkList from './components/LinkList'; // 1. Importar LinkList

function App() {
  const [links, setLinks] = useState([/*...*/]);

  const handleAddLink = (title, url) => { /*...*/ };
  const handleDeleteLink = (id) => { /*...*/ };

  return (
    <div className="App">
      <section className="profile-header">{/*...*/}</section>

      <AddLinkForm onAddLink={handleAddLink} />

      {/* 2. Usar el nuevo componente LinkList */}
      <LinkList links={links} onDelete={handleDeleteLink} />

    </div>
  );
}

export default App;
```

**¿Por qué y qué hace?**

*   ¡Mira qué limpio ha quedado el `return` de `App.jsx`! Ahora es muy fácil de leer y entender la estructura de alto nivel de nuestra aplicación.
*   `App.jsx` ahora se enfoca en sus responsabilidades de **contenedor**: manejar el estado `links` y definir las funciones `handleAddLink` y `handleDeleteLink`.
*   Luego, pasa los datos y las funciones necesarias a los componentes de **presentación** (`AddLinkForm` y `LinkList`), que se encargan del resto.

Este patrón de **Contenedor/Presentación** es una de las claves para escribir aplicaciones React mantenibles y escalables. Te permite separar la lógica de la vista, lo que hace que ambos sean más fáciles de entender, probar y modificar de forma independiente.
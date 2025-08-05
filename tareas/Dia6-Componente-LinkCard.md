# Día 6: Mejorando el Componente `LinkCard`

## Objetivos del Día

-   Añadir un botón de "Eliminar" a cada `LinkCard`.
-   Entender por qué la lógica de eliminación no puede vivir (todavía) dentro de `LinkCard`.
-   Pasar funciones como `props` de un componente padre a un hijo.

## Tareas

### 1. Añadir un botón de Eliminar a `LinkCard.jsx`

Nuestro objetivo es poder eliminar enlaces individualmente. El primer paso es añadir un botón para esta acción en cada tarjeta de enlace.

**Paso a paso:**

1.  Abre el archivo del componente `src/components/LinkCard.jsx`.
2.  Dentro del `div` principal, al lado de la etiqueta `<a>`, añade una etiqueta `<button>`.

**Código a modificar en `LinkCard.jsx`:**

```jsx
import React from 'react';

function LinkCard(props) {
  return (
    <div className="link-card">
      <a href={props.url} target="_blank" rel="noopener noreferrer">
        {props.title}
      </a>
      {/* Añadir este botón */}
      <button>Eliminar</button>
    </div>
  );
}

export default LinkCard;
```

**¿Por qué y qué hace?**

*   **`<button>Eliminar</button>`**: Simplemente estamos añadiendo un botón visible a cada una de nuestras tarjetas de enlace. Por ahora, este botón no hace nada, pero es el elemento de la interfaz de usuario con el que el usuario interactuará para eliminar un enlace.

### 2. El problema: ¿Dónde vive el estado?

Ahora, ¿cómo hacemos que ese botón funcione? Nuestra primera inclinación podría ser añadir una función `handleDelete` dentro de `LinkCard.jsx`.

**Pero aquí hay un problema fundamental:**

-   El componente `LinkCard` **no conoce la lista completa de enlaces**. Solo recibe `title` y `url` para un único enlace a través de `props`.
-   La lista completa de enlaces (el array `links`) vive en el **estado del componente padre, `App.jsx`**.
-   Para eliminar un enlace, necesitamos modificar ese array. Por lo tanto, la lógica de eliminación **debe** estar en el componente que es dueño del estado, es decir, en `App.jsx`.

Esto nos lleva a un patrón increíblemente común e importante en React: **"Lifting State Up"** (Levantar el Estado), que veremos en detalle en la siguiente tarea. Por ahora, lo que necesitamos es una forma de que el `LinkCard` (el hijo) le diga a `App.jsx` (el padre): "¡Oye, quiero que me elimines!".

### 3. Crear la función de eliminación en `App.jsx`

Vamos a crear la función que realmente hace el trabajo de eliminar en el componente que tiene el poder para hacerlo: `App.jsx`.

**Paso a paso:**

1.  Abre `src/App.jsx`.
2.  Dentro de la función `App`, crea una nueva función llamada `handleDeleteLink` que acepte un `id` como argumento.

**Código a añadir en `App.jsx`:**

```jsx
function App() {
  const [links, setLinks] = useState([/*...*/]);
  // ... otros estados y funciones ...

  // Nueva función para eliminar un enlace
  const handleDeleteLink = (idToDelete) => {
    // Filtramos el array, creando uno nuevo que excluye el enlace con el id a eliminar
    const updatedLinks = links.filter(link => link.id !== idToDelete);
    // Actualizamos el estado con el nuevo array
    setLinks(updatedLinks);
  };

  // ... return ...
}
```

**¿Por qué y qué hace?**

*   **`handleDeleteLink = (idToDelete) => { ... }`**: Definimos una función que toma un argumento, `idToDelete`, que será el `id` del enlace que queremos eliminar.
*   **`links.filter(link => link.id !== idToDelete)`**: Este es el corazón de la lógica de eliminación inmutable.
    *   El método `.filter()` de los arrays crea un **nuevo array** con todos los elementos que pasan una prueba (es decir, para los que la función devuelve `true`).
    *   `link => link.id !== idToDelete`: Por cada `link` en el array `links`, comprobamos si su `id` es **diferente** al `idToDelete`.
    *   Si el `id` es diferente, la condición es `true` y el `link` se incluye en el nuevo array `updatedLinks`.
    *   Si el `id` es igual, la condición es `false` y el `link` se omite (se filtra).
*   **`setLinks(updatedLinks)`**: Actualizamos el estado con el nuevo array que ya no contiene el enlace eliminado. React se encargará de volver a renderizar la lista.

### 4. Pasar la función de eliminación como `prop`

Ahora que la función existe en `App.jsx`, necesitamos dársela al componente `LinkCard` para que pueda usarla.

**Paso a paso:**

1.  En `App.jsx`, busca donde renderizas la lista de `LinkCard` con `.map()`.
2.  Añade una nueva `prop` al componente `<LinkCard>`, por ejemplo `onDelete`, y pásale la función `handleDeleteLink`.

**Código a modificar en `App.jsx`:**

```jsx
<section className="links-container">
  {links.map(link => (
    <LinkCard
      key={link.id}
      title={link.title}
      url={link.url}
      // ¡Pasando la función como prop!
      onDelete={handleDeleteLink}
    />
  ))}
</section>
```

**¿Por qué y qué hace?**

*   **`onDelete={handleDeleteLink}`**: ¡Puedes pasar funciones como `props`! Esto es extremadamente poderoso. Le estamos dando al componente hijo `LinkCard` una "herramienta" (la función `handleDeleteLink`) que fue creada en el padre.
*   La convención es nombrar las `props` que son funciones de eventos con `on[Evento]`, como `onDelete`, `onClick`, `onUpdate`, etc.

En la siguiente tarea, veremos cómo el `LinkCard` recibe y utiliza esta `prop` para finalmente conectar el botón de "Eliminar".
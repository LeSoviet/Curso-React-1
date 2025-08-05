# Día 6: "Lifting State Up" (Levantando el Estado)

## Objetivos del Día

-   Entender el concepto de "Lifting State Up".
-   Conectar la función de eliminación del padre (`App.jsx`) con el botón del hijo (`LinkCard.jsx`).
-   Hacer que el botón "Eliminar" sea completamente funcional.

## Tareas

### 1. ¿Qué es "Lifting State Up"?

Es uno de los patrones más importantes en React. La idea es la siguiente:

> Cuando varios componentes necesitan compartir y manipular el mismo estado, ese estado debe moverse al ancestro común más cercano en el árbol de componentes.

En nuestro caso:

-   `App.jsx` tiene el estado (el array `links`).
-   `LinkCard.jsx` necesita **manipular** ese estado (eliminando un elemento).

Como `LinkCard` no puede modificar directamente el estado de su padre, "levantamos el estado". Esto no significa que movemos el estado en sí, sino que **la lógica para modificar el estado (`handleDeleteLink`) se mantiene en el padre (`App.jsx`), y el padre le pasa esa lógica al hijo como una prop (`onDelete`).**

El hijo simplemente invoca esa función cuando es necesario, sin saber los detalles de cómo funciona. Solo sabe que al llamarla, algo sucederá.

### 2. Recibir y usar las `props` en `LinkCard.jsx`

En la tarea anterior, pasamos la función `handleDeleteLink` como una `prop` llamada `onDelete` a cada `LinkCard`. Ahora, vamos a recibirla y usarla.

**Paso a paso:**

1.  Abre el archivo `src/components/LinkCard.jsx`.
2.  El botón "Eliminar" necesita saber **qué** enlace eliminar. Para ello, también debemos pasar el `id` del enlace como `prop` desde `App.jsx`.

    *   **Primero, modifica `App.jsx`** para pasar el `id`:

        ```jsx
        // En App.jsx
        <LinkCard
          key={link.id}
          id={link.id} // <-- Añade esta línea
          title={link.title}
          url={link.url}
          onDelete={handleDeleteLink}
        />
        ```

3.  Ahora sí, en `LinkCard.jsx`, vamos a conectar todo al evento `onClick` del botón.

**Código final de `LinkCard.jsx`:**

```jsx
import React from 'react';

// Recibimos las nuevas props: id y onDelete
function LinkCard({ id, title, url, onDelete }) {
  const handleDelete = () => {
    // Llamamos a la función que recibimos por props, pasándole el id
    onDelete(id);
  };

  return (
    <div className="link-card">
      <a href={url} target="_blank" rel="noopener noreferrer">
        {title}
      </a>
      {/* El botón ahora llama a nuestra función local handleDelete */}
      <button onClick={handleDelete}>Eliminar</button>
    </div>
  );
}

export default LinkCard;
```

**¿Por qué y qué hace?**

*   **`function LinkCard({ id, title, url, onDelete })`**: Estamos usando la "desestructuración de props" aquí. En lugar de escribir `props` y luego `props.id`, `props.title`, etc., podemos extraer las propiedades directamente en la firma de la función. Es una sintaxis más limpia y común.

*   **`const handleDelete = () => { ... }`**: Hemos creado una pequeña función de envoltura dentro de `LinkCard`. Esto no es estrictamente necesario (podríamos haber puesto la lógica directamente en el `onClick`), pero hace que el código sea más legible.

*   **`onDelete(id)`**: ¡Esta es la conexión clave! Estamos llamando a la función `onDelete` que recibimos a través de las `props`. Recuerda, `onDelete` es en realidad la función `handleDeleteLink` de `App.jsx`. Al llamarla, le pasamos el `id` que también recibimos por `props`. Esto le dice a la función en `App.jsx` *cuál* es el enlace que debe ser eliminado.

*   **`onClick={handleDelete}`**: El atributo `onClick` espera una función. Cuando el usuario hace clic en el botón, React ejecutará la función que le pasemos. En este caso, ejecuta nuestra función `handleDelete`, que a su vez llama a la función del padre.

### Resumen del Flujo de Datos

Veamos el viaje completo de un clic:

1.  **Usuario Clica**: Haces clic en el botón "Eliminar" de un `LinkCard`.
2.  **Evento `onClick`**: El `onClick` en el `<button>` del `LinkCard.jsx` se dispara.
3.  **Llamada a la Prop**: Se ejecuta la función `handleDelete`, que llama a `onDelete(id)`, pasando el `id` de ese enlace específico (ej: `onDelete(2)`).
4.  **Ejecución en el Padre**: La llamada a `onDelete(2)` en el hijo ejecuta la función `handleDeleteLink(2)` en el padre (`App.jsx`).
5.  **Filtrado de Estado**: `handleDeleteLink` filtra el array `links`, creando un nuevo array sin el enlace que tiene `id: 2`.
6.  **Actualización de Estado**: La función llama a `setLinks()` con el nuevo array.
7.  **Re-renderizado de React**: React detecta que el estado `links` ha cambiado. Vuelve a renderizar el componente `App` y sus hijos. Como el array `links` ahora es más corto, el `.map()` renderizará un `LinkCard` menos.
8.  **UI Actualizada**: El enlace eliminado desaparece de la pantalla.

¡Felicidades! Has implementado uno de los patrones más fundamentales para construir aplicaciones interactivas en React.

# Día 7: Manejo del Envío del Formulario y "Lifting State Up"

## Objetivos del Día

-   Crear la función para añadir un nuevo enlace en el componente padre (`App.jsx`).
-   Pasar esa función como `prop` al componente del formulario (`AddLinkForm.jsx`).
-   Manejar el evento de envío del formulario para añadir el nuevo enlace a la lista.

## Tareas

### 1. El mismo patrón: "Lifting State Up"

Nos encontramos en una situación idéntica a la del botón "Eliminar":

-   El **estado** que necesita ser modificado (el array `links`) vive en `App.jsx`.
-   La **acción** que desencadena la modificación (hacer clic en "Añadir") ocurre en un componente hijo, `AddLinkForm.jsx`.

La solución es la misma: la lógica para añadir un enlace debe vivir en `App.jsx`, y pasaremos esa lógica al hijo a través de `props`.

### 2. Crear la función `handleAddLink` en `App.jsx`

Esta función se encargará de recibir los datos del nuevo enlace (título y URL) y añadirlos al estado `links`.

**Paso a paso:**

1.  Abre `src/App.jsx`.
2.  Debajo de tu función `handleDeleteLink`, crea una nueva función llamada `handleAddLink`.

**Código a añadir en `App.jsx`:**

```jsx
function App() {
  const [links, setLinks] = useState([/*...*/]);
  // ...

  const handleDeleteLink = (idToDelete) => { /*...*/ };

  // Nueva función para AÑADIR un enlace
  const handleAddLink = (newTitle, newUrl) => {
    const newLink = {
      // Para el id, usamos la fecha actual para garantizar que sea único (una solución simple)
      id: Date.now(),
      title: newTitle,
      url: newUrl,
    };
    // Creamos un nuevo array que contiene todos los links antiguos más el nuevo
    const updatedLinks = [...links, newLink];
    // Actualizamos el estado
    setLinks(updatedLinks);
  };

  return ( /*...*/ );
}
```

**¿Por qué y qué hace?**

*   **`handleAddLink = (newTitle, newUrl) => { ... }`**: Definimos una función que acepta el título y la URL del nuevo enlace como argumentos.
*   **`const newLink = { ... }`**: Creamos un nuevo objeto de enlace con los datos recibidos.
    *   `id: Date.now()`: Necesitamos un `id` único para la `key` de React. `Date.now()` devuelve el número de milisegundos desde una fecha de referencia, lo que nos da un número que es prácticamente siempre único para este caso de uso.
*   **`const updatedLinks = [...links, newLink]`**: Esta es la forma moderna e **inmutable** de añadir un elemento a un array en JavaScript.
    *   `...links`: Esto se llama "spread syntax". Lo que hace es "esparcir" o "desempacar" todos los elementos del array `links` y ponerlos en el nuevo array.
    *   `, newLink`: Después de todos los elementos antiguos, añadimos el `newLink` al final.
    *   El resultado es un **nuevo array** `updatedLinks` que contiene todo lo de antes más el nuevo elemento.
*   **`setLinks(updatedLinks)`**: Actualizamos el estado con este nuevo array, y React se encargará de renderizar el nuevo enlace en la pantalla.

### 3. Pasar `handleAddLink` como `prop`

Ahora, le daremos al formulario la herramienta que necesita para comunicarse con el padre.

**Paso a paso:**

1.  En el `return` de `App.jsx`, busca donde usas el componente `<AddLinkForm />`.
2.  Pásale la función `handleAddLink` como una `prop`. La convención es llamarla `onAddLink`.

**Código a modificar en `App.jsx`:**

```jsx
// ...
return (
  <div className="App">
    <section className="profile-header">{/*...*/}</section>

    <AddLinkForm onAddLink={handleAddLink} />

    <section className="links-container">{/*...*/}</section>
  </div>
);
// ...
```

### 4. Manejar el envío en `AddLinkForm.jsx`

Es hora de conectar todo. Recibiremos la `prop` `onAddLink` y la llamaremos cuando el formulario se envíe.

**Paso a paso:**

1.  Abre `src/components/AddLinkForm.jsx`.
2.  Recibe la `prop` `onAddLink`.
3.  Crea una función `handleSubmit` que se ejecutará cuando el formulario se envíe.
4.  Llama a `handleSubmit` desde el evento `onSubmit` del formulario.

**Código final de `AddLinkForm.jsx`:**

```jsx
import React, { useState } from 'react';

// 2. Recibir la prop onAddLink
function AddLinkForm({ onAddLink }) {
  const [title, setTitle] = useState('');
  const [url, setUrl] = useState('');

  // 3. Crear la función handleSubmit
  const handleSubmit = (e) => {
    // Previene que la página se recargue, que es el comportamiento por defecto del formulario
    e.preventDefault();

    // Validación simple: no añadir si los campos están vacíos
    if (!title.trim() || !url.trim()) {
      alert('Por favor, completa ambos campos.');
      return;
    }

    // Llamamos a la función del padre, pasándole los valores de nuestros estados locales
    onAddLink(title, url);

    // Limpiamos los campos del formulario después de añadir
    setTitle('');
    setUrl('');
  };

  return (
    // 4. Conectar la función al evento onSubmit del formulario
    <form onSubmit={handleSubmit}>
      <h2>Añadir Nuevo Enlace</h2>
      <input
        type="text"
        placeholder="Título del enlace"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
      />
      <input
        type="url"
        placeholder="URL del enlace"
        value={url}
        onChange={(e) => setUrl(e.target.value)}
      />
      <button type="submit">Añadir</button>
    </form>
  );
}

export default AddLinkForm;
```

**¿Por qué y qué hace?**

*   **`onSubmit={handleSubmit}`**: En lugar de un `onClick` en el botón, es mejor práctica usar el evento `onSubmit` del formulario. Esto captura el envío tanto si se hace clic en el botón como si se presiona "Enter" en un campo.
*   **`e.preventDefault()`**: Esta línea es **crucial**. Le dice al navegador: "No hagas lo que normalmente harías al enviar un formulario (que es recargar la página)". Nosotros controlaremos lo que pasa a continuación con JavaScript.
*   **`onAddLink(title, url)`**: Aquí está la magia. Llamamos a la función que nos pasó el padre (`App.jsx`), enviándole el `title` y la `url` que hemos estado guardando en el estado de este componente.
*   **`setTitle(''); setUrl('');`**: Una vez que el enlace ha sido enviado, es una buena práctica de experiencia de usuario limpiar los campos del formulario para que el usuario pueda añadir otro enlace fácilmente.

¡Y listo! Ahora tienes un formulario completamente funcional. Cuando escribes, el estado local de `AddLinkForm` se actualiza. Cuando envías, llamas a una función del padre, que actualiza el estado global de la aplicación, y React se encarga de que veas el nuevo enlace en la pantalla. Has dominado el flujo de datos de React.

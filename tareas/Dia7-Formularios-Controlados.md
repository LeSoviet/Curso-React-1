# Día 7: Formularios y Componentes Controlados

## Objetivos del Día

-   Crear un formulario para añadir nuevos enlaces.
-   Entender el concepto de "Componentes Controlados" en React.
-   Manejar el estado de los inputs de un formulario.

## Tareas

### 1. Crear un nuevo componente: `AddLinkForm.jsx`

Para mantener nuestro `App.jsx` limpio, vamos a crear un componente separado para el formulario.

**Paso a paso:**

1.  En tu carpeta `src/components`, crea un nuevo archivo llamado `AddLinkForm.jsx`.
2.  Crea la estructura básica del componente con un formulario que contenga dos `inputs` (para el título y la URL) y un botón de `submit`.

**Código inicial para `AddLinkForm.jsx`:**

```jsx
import React from 'react';

function AddLinkForm() {
  return (
    <form>
      <h2>Añadir Nuevo Enlace</h2>
      <input
        type="text"
        placeholder="Título del enlace"
      />
      <input
        type="url"
        placeholder="URL del enlace"
      />
      <button type="submit">Añadir</button>
    </form>
  );
}

export default AddLinkForm;
```

**¿Por qué y qué hace?**

*   **`<form>`**: La etiqueta semántica para crear un formulario.
*   **`<input type="text">`**: Un campo de entrada para texto simple, como el título.
*   **`<input type="url">`**: Un tipo especial de campo de entrada para URLs. En algunos dispositivos móviles, mostrará un teclado optimizado para escribir URLs.
*   **`placeholder`**: Un texto de ayuda que aparece en el campo de entrada cuando está vacío.
*   **`<button type="submit">`**: Un botón que, por defecto, al ser presionado dentro de un `<form>`, intentará enviar los datos y recargar la página. Prevendremos este comportamiento más adelante.

### 2. Introducción a los Componentes Controlados

En HTML normal, los formularios "recuerdan" lo que escribes en ellos. Tienen su propio estado interno.

En React, preferimos un enfoque diferente: **el estado del componente React es la única fuente de verdad**. Esto significa que en lugar de dejar que el DOM maneje el estado del input, lo vamos a manejar nosotros mismos usando el Hook `useState`.

Un **componente controlado** es un componente de formulario (como un `<input>`) cuyo valor es controlado por el estado de React.

**El ciclo es así:**
1.  El valor del input se guarda en el estado de React.
2.  El input en el JSX muestra el valor que está en el estado.
3.  Cuando el usuario escribe en el input, se dispara un evento `onChange`.
4.  La función `onChange` actualiza el estado de React con el nuevo valor.
5.  Como el estado cambió, el componente se re-renderiza y el input muestra el nuevo valor del estado.

### 3. Implementar el Componente Controlado

**Paso a paso:**

1.  **Importa `useState`** en `AddLinkForm.jsx`.
2.  Crea **dos piezas de estado**: una para el título y otra para la URL.
3.  Conecta estas piezas de estado a los inputs usando los atributos `value` y `onChange`.

**Código a modificar en `AddLinkForm.jsx`:**

```jsx
import React, { useState } from 'react'; // 1. Importar useState

function AddLinkForm() {
  // 2. Crear piezas de estado
  const [title, setTitle] = useState('');
  const [url, setUrl] = useState('');

  return (
    <form>
      <h2>Añadir Nuevo Enlace</h2>
      <input
        type="text"
        placeholder="Título del enlace"
        value={title} // 3a. Conectar el valor al estado
        onChange={(e) => setTitle(e.target.value)} // 3b. Actualizar el estado al cambiar
      />
      <input
        type="url"
        placeholder="URL del enlace"
        value={url} // 3a. Conectar el valor al estado
        onChange={(e) => setUrl(e.target.value)} // 3b. Actualizar el estado al cambiar
      />
      <button type="submit">Añadir</button>
    </form>
  );
}

export default AddLinkForm;
```

**¿Por qué y qué hace?**

*   **`const [title, setTitle] = useState('');`**: Creamos una variable de estado llamada `title` para almacenar el valor del input de título. Su valor inicial es un string vacío `''`.
*   **`value={title}`**: Le estamos diciendo al input: "Tu valor siempre será lo que sea que esté en la variable de estado `title`". Esta es la parte que hace que React "controle" el input.
*   **`onChange={...}`**: El evento `onChange` se dispara cada vez que el usuario escribe o borra un carácter en el input.
*   **`(e) => setTitle(e.target.value)`**: Esta es la función que se ejecuta en el `onChange`.
    *   `e`: Es el objeto del evento. Contiene información sobre lo que acaba de pasar.
    *   `e.target`: Es el elemento del DOM que disparó el evento (en este caso, el `<input>`).
    *   `e.target.value`: Es el valor actual del contenido del input.
    *   `setTitle(e.target.value)`: Llamamos a la función de actualización del estado para cambiar `title` al valor actual del input. Esto completa el ciclo.

### 4. Integrar el formulario en `App.jsx`

Finalmente, vamos a mostrar nuestro nuevo formulario en la página principal.

**Paso a paso:**

1.  Abre `src/App.jsx`.
2.  Importa el componente `AddLinkForm`.
3.  Coloca el componente `<AddLinkForm />` en algún lugar de tu JSX, por ejemplo, encima de la lista de enlaces.

**Código a añadir/modificar en `App.jsx`:**

```jsx
import './App.css';
import { useState } from 'react';
import LinkCard from './components/LinkCard';
import AddLinkForm from './components/AddLinkForm'; // 1. Importar

function App() {
  // ... estados y funciones ...

  return (
    <div className="App">
      <section className="profile-header">{/*...*/}</section>

      {/* 2. Añadir el componente del formulario */}
      <AddLinkForm />

      <section className="links-container">{/*...*/}</section>
    </div>
  );
}

export default App;
```

Ahora deberías ver tu formulario en la página. Si escribes en los campos, notarás que funciona como un input normal. Sin embargo, detrás de escena, has implementado un patrón fundamental de React. El siguiente paso será hacer que el botón "Añadir" realmente añada el nuevo enlace a nuestra lista, lo que nos llevará de nuevo al patrón de "Lifting State Up".
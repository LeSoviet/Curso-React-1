# Día 2: Componentes y Props

## Objetivos del Día

- Crear un componente reutilizable para los enlaces.
- Pasar datos a los componentes a través de `props`.
- Renderizar una lista de enlaces.

## Tareas

### 1. Crear un nuevo componente: `LinkCard`

**Paso a paso:**

1.  Dentro de la carpeta `src`, crea una nueva carpeta llamada `components`.
    *   Haz clic derecho en la carpeta `src` -> `Nueva carpeta` -> nombra la carpeta `components`.
2.  Dentro de la carpeta `components`, crea un nuevo archivo llamado `LinkCard.jsx`.
    *   Haz clic derecho en la carpeta `components` -> `Nuevo archivo` -> nombra el archivo `LinkCard.jsx`.

**¿Por qué y qué hace?**

*   **¿Por qué una carpeta `components`?** Es una convención común en proyectos de React organizar los componentes reutilizables en una carpeta dedicada. Esto mantiene nuestro código ordenado y fácil de navegar a medida que la aplicación crece.
*   **¿Qué es `LinkCard.jsx`?** Este archivo contendrá nuestro componente `LinkCard`. La idea es que este componente sea responsable de mostrar un único enlace. Al crear un componente separado, podemos reutilizarlo para cada enlace que queramos mostrar, en lugar de repetir el mismo código una y otra vez.

### 2. Escribir el código del componente `LinkCard`

**Paso a paso:**

1.  Abre el archivo `src/components/LinkCard.jsx`.
2.  Copia y pega el siguiente código dentro de él:

```jsx
import React from 'react';

function LinkCard(props) {
  return (
    <div className="link-card">
      <a href={props.url} target="_blank" rel="noopener noreferrer">
        {props.title}
      </a>
    </div>
  );
}

export default LinkCard;
```

**¿Por qué y qué hace?**

*   **`import React from 'react';`**: Esta línea importa la biblioteca de React, que es necesaria para que nuestros componentes funcionen correctamente.
*   **`function LinkCard(props) { ... }`**: Esto define nuestro componente `LinkCard`. Es una función de JavaScript que devuelve JSX.
*   **`props`**: Fíjate en el parámetro `props`. `props` (abreviatura de "properties") es un objeto que contiene todos los datos que pasamos a un componente desde su padre. Es la forma en que los componentes se comunican y reciben información.
*   **`className="link-card"`**: Asignamos una clase de CSS para poder darle estilo a nuestras tarjetas de enlace más adelante.
*   **`<a href={props.url} ...>`**: Esta es una etiqueta de anclaje (un enlace).
    *   `href={props.url}`: El atributo `href` especifica la URL a la que el enlace debe dirigir. En lugar de una URL fija, usamos `{props.url}`. Esto significa que el valor de la URL vendrá de las `props` que le pasemos al componente. Las llaves `{}` nos permiten incrustar expresiones de JavaScript directamente en nuestro JSX.
    *   `target="_blank"`: Esto le dice al navegador que abra el enlace en una nueva pestaña.
    *   `rel="noopener noreferrer"`: Estos son atributos de seguridad importantes cuando se usa `target="_blank"`.
*   **`{props.title}`**: Dentro de la etiqueta `<a>`, mostramos el texto del enlace. Al igual que con la URL, el texto del título también vendrá de las `props`.
*   **`export default LinkCard;`**: Esta línea hace que nuestro componente `LinkCard` esté disponible para ser importado y utilizado en otras partes de nuestra aplicación, como en `App.jsx`.

### 3. Usar el componente `LinkCard` en `App.jsx`

**Paso a paso:**

1.  Abre el archivo `src/App.jsx`.
2.  **Importa** el componente `LinkCard` en la parte superior del archivo, debajo de las otras importaciones:
    ```jsx
    import LinkCard from './components/LinkCard';
    ```
3.  **Añade una sección** para los enlaces debajo de la sección del perfil.
4.  **Usa el componente `LinkCard`** dentro de la nueva sección, pasándole las `props` `title` y `url`.

**Código final de `App.jsx`:**

```jsx
import './App.css';
import LinkCard from './components/LinkCard'; // <-- Importación

function App() {
  return (
    <div className="App">
      <section className="profile-header">
        <img src="https://via.placeholder.com/150" alt="Profile" />
        <h1>John Doe</h1>
        <p>Software Developer | React Enthusiast</p>
      </section>

      {/* Nueva sección de enlaces */}
      <section className="links-container">
        <LinkCard title="Mi Portafolio" url="https://github.com/johndoe" />
        <LinkCard title="Twitter" url="https://twitter.com/johndoe" />
        <LinkCard title="LinkedIn" url="https://linkedin.com/in/johndoe" />
      </section>
    </div>
  );
}

export default App;
```

**¿Por qué y qué hace?**

*   **`import LinkCard from './components/LinkCard';`**: Esta línea importa el componente que acabamos de crear, haciéndolo disponible para su uso en `App.jsx`.
*   **`<section className="links-container">`**: Creamos otra sección para agrupar nuestros enlaces, lo que facilitará la aplicación de estilos más adelante.
*   **`<LinkCard title="Mi Portafolio" url="https://github.com/johndoe" />`**: Así es como usamos nuestro componente `LinkCard`.
    *   `LinkCard`: Es el nombre del componente que estamos usando.
    *   `title="Mi Portafolio"`: Esto es una `prop`. Estamos pasando el string `"Mi Portafolio"` como el valor de la `prop` `title`. Dentro del componente `LinkCard`, `props.title` será igual a `"Mi Portafolio"`.
    *   `url="https://github.com/johndoe"`: De manera similar, estamos pasando la URL como la `prop` `url`. Dentro de `LinkCard`, `props.url` será `"https://github.com/johndoe"`.

Al usar componentes y `props`, hemos creado una forma reutilizable y limpia de mostrar enlaces. Si queremos añadir otro enlace, simplemente añadimos otra línea `<LinkCard ... />` con un `title` y `url` diferentes, sin tener que duplicar todo el código HTML del enlace cada vez.
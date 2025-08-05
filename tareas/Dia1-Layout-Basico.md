# Día 1: Layout Básico y JSX

## Objetivos del Día

- Crear la estructura básica de la página de perfil.
- Añadir una imagen de perfil, un nombre de usuario y una breve descripción.
- Utilizar JSX para estructurar el contenido.

## Tareas

### 1. Abrir `src/App.jsx`

**Paso a paso:**

1.  En tu editor de código, busca la carpeta `src` en el panel de la izquierda.
2.  Haz clic en la carpeta `src` para expandirla.
3.  Dentro de la carpeta `src`, encontrarás el archivo `App.jsx`. Haz doble clic en él para abrirlo.

**¿Por qué y qué hace?**

*   **¿Qué es `App.jsx`?** Es el componente principal de tu aplicación de React. Piensa en los componentes como bloques de construcción para tu interfaz de usuario. `App.jsx` es el bloque de construcción raíz que contendrá todos los demás.
*   **¿Por qué `jsx`?** La extensión `.jsx` significa que el archivo contiene sintaxis JSX. JSX es una extensión de JavaScript que te permite escribir código similar a HTML dentro de tus archivos de JavaScript. Esto hace que la creación de interfaces de usuario sea más intuitiva.

### 2. Limpiar el contenido inicial

**Paso a paso:**

1.  Dentro de `App.jsx`, localiza la función `App`. Verás una sección que comienza con `return (`.
2.  Elimina todo el código que se encuentra entre los paréntesis de `return ()`.

**Antes:**

```jsx
function App() {
  const [count, setCount] = useState(0)

  return (
    <>
      <div>
        <a href="https://vitejs.dev" target="_blank">
          <img src={viteLogo} className="logo" alt="Vite logo" />
        </a>
        <a href="https://react.dev" target="_blank">
          <img src={reactLogo} className="logo react" alt="React logo" />
        </a>
      </div>
      <h1>Vite + React</h1>
      <div className="card">
        <button onClick={() => setCount((count) => count + 1)}>
          count is {count}
        </button>
        <p>
          Edit <code>src/App.jsx</code> and save to test HMR
        </p>
      </div>
      <p className="read-the-docs">
        Click on the Vite and React logos to learn more
      </p>
    </>
  )
}
```

**Después:**

```jsx
function App() {
  return (
    // El contenido irá aquí
  )
}
```

**¿Por qué y qué hace?**

*   **¿Qué estamos haciendo?** Estamos eliminando el contenido de demostración que viene con la plantilla de React. Esto nos da un lienzo en blanco para construir nuestra propia interfaz.
*   **¿Por qué es importante?** Es fundamental eliminar el código innecesario para mantener nuestro proyecto limpio y organizado desde el principio.

### 3. Añadir la estructura del perfil

**Paso a paso:**

1.  Dentro de los paréntesis de `return ()`, añade los siguientes elementos:

    *   **Imagen de perfil:** Copia y pega la siguiente línea:
        ```jsx
        <img src="https://via.placeholder.com/150" alt="Profile" />
        ```
    *   **Nombre de usuario:** Debajo de la imagen, copia y pega:
        ```jsx
        <h1>John Doe</h1>
        ```
    *   **Descripción:** Debajo del nombre de usuario, copia y pega:
        ```jsx
        <p>Software Developer | React Enthusiast</p>
        ```

**Código completo hasta ahora:**

```jsx
function App() {
  return (
    <>
      <img src="https://via.placeholder.com/150" alt="Profile" />
      <h1>John Doe</h1>
      <p>Software Developer | React Enthusiast</p>
    </>
  )
}
```

**¿Por qué y qué hace?**

*   **`<img ... />`**: Esta es una etiqueta de imagen.
    *   `src="https://via.placeholder.com/150"`: El atributo `src` (source) le dice a la etiqueta de imagen dónde encontrar la imagen a mostrar. Aquí estamos usando una URL de un servicio de imágenes de marcador de posición.
    *   `alt="Profile"`: El atributo `alt` (alternative text) proporciona un texto descriptivo para la imagen. Esto es importante para la accesibilidad (los lectores de pantalla lo usan) y también se muestra si la imagen no se puede cargar.
*   **`<h1>...</h1>`**: Esta es una etiqueta de encabezado de nivel 1. Se utiliza para los títulos más importantes de una página. En este caso, el nombre de nuestro perfil.
*   **`<p>...</p>`**: Esta es una etiqueta de párrafo. Se utiliza para texto normal, como nuestra descripción.
*   **`<> ... </>`**: Esto se llama un "Fragment". En React, un componente debe devolver un único elemento raíz. Si quieres devolver varios elementos (como nuestra imagen, encabezado y párrafo), debes envolverlos en un elemento contenedor. Un Fragment te permite hacer esto sin agregar un nodo extra al DOM (la estructura de la página web).

### 4. Envolver todo en un `section`

**Paso a paso:**

1.  Ahora, vamos a agrupar nuestros elementos de perfil dentro de una etiqueta `<section>`. Esto es útil para aplicar estilos a todo el grupo más adelante.
2.  Reemplaza los Fragments (`<>` y `</>`) con etiquetas `<section>` y `</section>`.
3.  Añade un `className` a la etiqueta `<section>`.

**Código final:**

```jsx
function App() {
  return (
    <section className="profile-header">
      <img src="https://via.placeholder.com/150" alt="Profile" />
      <h1>John Doe</h1>
      <p>Software Developer | React Enthusiast</p>
    </section>
  )
}
```

**¿Por qué y qué hace?**

*   **`<section>`**: Es una etiqueta HTML semántica que agrupa contenido relacionado. Usar etiquetas semánticas como `<section>`, `<article>`, `<nav>`, etc., ayuda a los motores de búsqueda y a las tecnologías de asistencia a comprender la estructura de tu página.
*   **`className="profile-header"`**: En JSX, usamos `className` en lugar de `class` (que es una palabra reservada en JavaScript) para asignar clases de CSS a los elementos. Esto nos permitirá seleccionar esta sección con CSS y aplicarle estilos específicos más adelante.
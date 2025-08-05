# Día 1: Creando el Layout Básico

## Objetivo del Día

Hoy nos centraremos en crear la estructura visual principal de nuestra aplicación. No te preocupes por la funcionalidad todavía, el objetivo es que te familiarices con JSX y la estructura de un componente de React.

## Tarea

Tu misión es modificar el componente `App.jsx` para que muestre un layout básico de una página de perfil.

### Paso a Paso

1.  **Limpiar el Componente `App.jsx`:**
    *   Abre el archivo `src/App.jsx`.
    *   Verás que tiene un contenido de ejemplo. Bórralo todo y quédate solo con la estructura básica de un componente de React. Debería verse así:

    ```jsx
    import './App.css';

    function App() {
      return (
        <div className="App">
          {/* Aquí irá nuestro código */}
        </div>
      );
    }

    export default App;
    ```

2.  **Crear la Estructura del Perfil:**
    *   Dentro del `div` con `className="App"`, añade los siguientes elementos usando JSX:
        *   Una imagen de perfil (puedes usar un placeholder de `https://via.placeholder.com/150`).
        *   Un `h1` para el nombre de usuario (ej: "@tunombredeusuario").
        *   Un `p` para una breve descripción o biografía.
        *   Una sección (`<section>`) que contendrá la lista de enlaces. Por ahora, déjala vacía.

3.  **Estilos (Opcional, pero recomendado):**
    *   Abre el archivo `src/App.css`.
    *   Limpia los estilos que vienen por defecto.
    *   Añade algunos estilos básicos para centrar el contenido de la página y darle un aspecto más ordenado. Por ejemplo:

    ```css
    .App {
      text-align: center;
      margin-top: 50px;
    }

    img {
      border-radius: 50%;
      width: 150px;
      height: 150px;
    }
    ```

4.  **Ver el Resultado:**
    *   Abre una terminal en la raíz del proyecto (`linkhub-project`).
    *   Ejecuta el comando `npm run dev`.
    *   Abre tu navegador y ve a la dirección que te indica la terminal (normalmente `http://localhost:5173`).
    *   Deberías ver tu layout básico en acción.

¡Mucho éxito! No dudes en preguntar si te atascas.

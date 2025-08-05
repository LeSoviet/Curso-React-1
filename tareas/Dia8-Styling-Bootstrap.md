# Día 8: Estilizando con un Framework CSS (Bootstrap)

## Objetivo del Día

Hoy le daremos a nuestra aplicación un aspecto mucho más profesional y limpio sin necesidad de escribir mucho CSS. Aprenderás a integrar un framework de CSS como Bootstrap en un proyecto de React.

## Tarea

Instalarás Bootstrap y usarás sus clases de utilidad para estilizar los componentes `ProfileHeader`, `LinkList` y `AddLinkForm`.

### Paso a Paso

1.  **Instalar Bootstrap:**
    *   Detén tu servidor de desarrollo (Ctrl + C en la terminal).
    *   Instala Bootstrap como una dependencia del proyecto ejecutando el siguiente comando en la terminal, dentro de la carpeta `linkhub-project`:

    ```bash
    npm install bootstrap
    ```

2.  **Importar el CSS de Bootstrap:**
    *   Para que los estilos de Bootstrap estén disponibles en toda tu aplicación, necesitas importar su archivo CSS principal.
    *   Abre `src/main.jsx` (el punto de entrada de tu aplicación) y añade la siguiente línea en la parte superior:

    ```javascript
    // src/main.jsx
    import 'bootstrap/dist/css/bootstrap.min.css';
    // ...resto de las importaciones
    ```

3.  **Estilizar los Componentes con Clases de Bootstrap:**
    *   Ahora puedes usar las clases de Bootstrap en el `className` de tus elementos JSX. Recuerda que en React se usa `className` en lugar de `class`.
    *   **En `App.jsx`:** Añade clases para crear un contenedor principal y centrarlo.
        ```jsx
        // <div className="App">
        <div className="container text-center mt-5">
        ```
    *   **En `ProfileHeader.jsx`:** Estiliza la imagen y el texto.
        ```jsx
        // <img ... />
        <img src={props.imageUrl} alt="Foto de perfil" className="rounded-circle img-thumbnail mb-3" />
        // <h1>
        <h1 className="h3">{props.username}</h1>
        // <p>
        <p className="text-muted">{props.bio}</p>
        ```
    *   **En `AddLinkForm.jsx`:** Estiliza el formulario para que se vea ordenado.
        ```jsx
        // <form>
        <form onSubmit={handleSubmit} className="d-flex gap-2 mt-4 mb-4">
        // <input title>
        <input ... className="form-control" />
        // <input url>
        <input ... className="form-control" />
        // <button>
        <button type="submit" className="btn btn-primary">Añadir</button>
        ```
    *   **En `LinkList.jsx`:** Convierte los enlaces en botones grandes y atractivos.
        ```jsx
        // <ul>
        <ul className="list-unstyled d-grid gap-3">
        // <li>
        <li key={link.id} className="d-flex justify-content-between align-items-center bg-light p-3 rounded">
        // <a>
        <a href={link.url} ... className="btn btn-outline-dark flex-grow-1 text-start">{link.title}</a>
        // <button>
        <button ... className="btn btn-danger btn-sm">Eliminar</button>
        ```

4.  **Reiniciar el Servidor y Ver los Cambios:**
    *   Vuelve a iniciar tu servidor de desarrollo:
    ```bash
    npm run dev
    ```
    *   Abre tu navegador. ¡Tu aplicación debería verse completamente diferente! Mucho más limpia y estructurada, gracias a Bootstrap.

Has aprendido a integrar una herramienta externa muy popular para acelerar el desarrollo del UI. Esto te permite enfocarte más en la lógica de React y menos en escribir CSS desde cero.
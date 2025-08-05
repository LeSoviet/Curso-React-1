# Día 5: Manejando Formularios y Entradas de Usuario

## Objetivo del Día

Hoy aprenderás a capturar la entrada del usuario a través de formularios. Haremos que nuestra función de "Añadir Enlace" sea mucho más útil permitiendo al usuario especificar el título y la URL del nuevo enlace a través de campos de texto.

## Tarea

Crearás un formulario con dos campos (`input`) para el título y la URL, y un botón de envío. Vincularás estos campos al estado del componente para crear lo que se conoce como "componentes controlados".

### Paso a Paso

1.  **Crear Estados para los Inputs:**
    *   En `src/App.jsx`, necesitarás dos nuevos estados para almacenar los valores de los campos de título y URL.

    ```javascript
    // src/App.jsx
    // ...al principio del componente App

    const [newTitle, setNewTitle] = useState('');
    const [newUrl, setNewUrl] = useState('');
    ```

2.  **Crear el Formulario JSX:**
    *   En el `return` de `App.jsx`, reemplaza el botón solitario de "Añadir Nuevo Enlace" por un formulario (`<form>`).
    *   Dentro del formulario, añade:
        *   Un `input` para el título.
        *   Un `input` para la URL.
        *   Un `button` de tipo `submit`.

3.  **Vincular Inputs al Estado (Componentes Controlados):**
    *   A cada `input`, añádele dos props:
        *   `value`: Vincula el valor del input a la variable de estado correspondiente (`newTitle` o `newUrl`).
        *   `onChange`: Esta función se dispara cada vez que el usuario escribe en el campo. La usaremos para actualizar el estado con el nuevo valor del input (`e.target.value`).

    ```jsx
    // src/App.jsx
    // ...en el return, reemplazando el botón anterior

    <form onSubmit={handleAddNewLink}>
      <input
        type="text"
        placeholder="Título del enlace"
        value={newTitle}
        onChange={(e) => setNewTitle(e.target.value)}
      />
      <input
        type="text"
        placeholder="URL del enlace"
        value={newUrl}
        onChange={(e) => setNewUrl(e.target.value)}
      />
      <button type="submit">Añadir Enlace</button>
    </form>
    ```

4.  **Manejar el Envío del Formulario:**
    *   Renombra tu función `handleAddLink` a `handleAddNewLink`.
    *   Esta función ahora recibirá el evento `e` del formulario.
    *   Lo primero que debe hacer es `e.preventDefault()` para evitar que el navegador recargue la página al enviar el formulario.
    *   Luego, en lugar de añadir un enlace quemado, crea el `newLink` usando los valores de los estados `newTitle` y `newUrl`.
    *   Después de añadir el enlace al estado `links`, limpia los campos del formulario reseteando los estados `newTitle` y `newUrl` a strings vacíos.

    ```javascript
    // src/App.jsx

    const handleAddNewLink = (e) => {
      e.preventDefault();
      if (!newTitle || !newUrl) return; // No añadir si los campos están vacíos

      const newLink = {
        id: Date.now(),
        title: newTitle,
        url: newUrl
      };

      setLinks([...links, newLink]);

      // Limpiar los inputs
      setNewTitle('');
      setNewUrl('');
    };
    ```

5.  **Probar el Formulario:**
    *   Ve a tu navegador. Ahora deberías ver dos campos de texto y un botón.
    *   Escribe un título y una URL, y haz clic en "Añadir Enlace". El nuevo enlace, con tus datos, debería aparecer en la lista, y los campos del formulario deberían limpiarse, listos para el siguiente enlace.

¡Felicidades! Has implementado una de las funcionalidades más comunes en cualquier aplicación web: capturar y procesar datos de un usuario.
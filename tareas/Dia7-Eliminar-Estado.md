# Día 7: Eliminando Elementos del Estado

## Objetivo del Día

Hoy completarás la funcionalidad básica de un CRUD (Create, Read, Update, Delete) aprendiendo a eliminar elementos de una lista en el estado. Añadirás un botón de "Eliminar" a cada enlace.

## Tarea

Pasarás una función desde `App.jsx` hasta el componente `LinkList.jsx` que permita eliminar un enlace específico de la lista basándose en su `id`.

### Paso a Paso

1.  **Crear la Función de Eliminación en `App.jsx`:**
    *   En `src/App.jsx`, crea una función llamada `handleDeleteLink` que acepte un `id` como argumento.
    *   Dentro de esta función, usarás el método `.filter()` de los arrays para crear un nuevo array que contenga todos los enlaces *excepto* el que tiene el `id` que quieres eliminar.
    *   Luego, actualizarás el estado con este nuevo array filtrado usando `setLinks()`.

    ```javascript
    // src/App.jsx
    // ...dentro del componente App

    const handleDeleteLink = (idToDelete) => {
      const updatedLinks = links.filter(link => link.id !== idToDelete);
      setLinks(updatedLinks);
    };
    ```

2.  **Pasar la Función como Prop a `LinkList`:**
    *   En el `return` de `App.jsx`, pasa la función `handleDeleteLink` como una prop a tu componente `LinkList`.

    ```jsx
    // src/App.jsx
    // ...en el return de App
    <LinkList links={links} onDeleteLink={handleDeleteLink} />
    ```

3.  **Modificar `LinkList` para Mostrar el Botón de Eliminar:**
    *   Abre `src/components/LinkList.jsx`.
    *   El componente ahora recibe la prop `onDeleteLink`.
    *   Dentro del `.map()`, al lado de cada enlace `<a>`, añade un botón de "Eliminar".
    *   Asígnale un evento `onClick`. Cuando se haga clic, debe llamar a `props.onDeleteLink()` y pasarle el `id` del enlace actual (`link.id`).

    ```jsx
    // src/components/LinkList.jsx

    function LinkList(props) {
      return (
        <section>
          <ul>
            {props.links.map(link => (
              <li key={link.id}>
                <a href={link.url} target="_blank" rel="noopener noreferrer">
                  {link.title}
                </a>
                <button onClick={() => props.onDeleteLink(link.id)}>Eliminar</button>
              </li>
            ))}
          </ul>
        </section>
      );
    }

    export default LinkList;
    ```
    *   **Nota importante:** Usamos una función de flecha `() => props.onDeleteLink(link.id)` en el `onClick`. Si solo pusiéramos `onClick={props.onDeleteLink(link.id)}`, la función se ejecutaría inmediatamente para cada enlace en cuanto el componente se renderice, eliminando todo. La función de flecha asegura que `onDeleteLink` solo se llame *cuando* se hace clic en el botón.

4.  **Probar la Funcionalidad:**
    *   Ve a tu navegador. Cada enlace en la lista ahora debería tener un botón de "Eliminar" al lado.
    *   Haz clic en uno de ellos. El enlace debería desaparecer de la lista.

¡Lo has logrado! Ahora tienes una mini-aplicación completamente funcional donde puedes añadir y eliminar elementos. Este patrón de pasar IDs y funciones de manejo de eventos es extremadamente común en React.
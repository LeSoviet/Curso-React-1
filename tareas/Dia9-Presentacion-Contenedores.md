# Día 9: Componentes de Presentación vs. Contenedores

## Objetivo del Día

Hoy aprenderás sobre un patrón de diseño común en React: separar los componentes en dos categorías: **Componentes Contenedores** (que se ocupan de *cómo funcionan las cosas*) y **Componentes de Presentación** (que se ocupan de *cómo se ven las cosas*).

Esto mejora la reutilización y la claridad del código. Un componente de presentación no tiene estado propio y recibe todos sus datos y funciones a través de props, haciéndolo muy predecible y fácil de probar.

## Tarea

Refactorizarás `LinkList.jsx`. Crearás un nuevo componente puramente de presentación llamado `LinkItem.jsx` que se encargará de renderizar una sola fila de enlace. `LinkList` se convertirá en un "contenedor" para los `LinkItem`.

### Paso a Paso

1.  **Crear el Componente de Presentación `LinkItem.jsx`:**
    *   En `src/components`, crea un nuevo archivo `LinkItem.jsx`.
    *   Este componente recibirá como props toda la información que necesita para un solo enlace: `title`, `url`, y el `id` del enlace. También recibirá la función `onDelete`.
    *   Copia el JSX de un solo `<li>` desde el `.map()` de `LinkList.jsx` a este nuevo componente.

    ```jsx
    // src/components/LinkItem.jsx

    function LinkItem({ id, title, url, onDelete }) { // Usamos desestructuración en las props
      return (
        <li className="d-flex justify-content-between align-items-center bg-light p-3 rounded">
          <a href={url} target="_blank" rel="noopener noreferrer" className="btn btn-outline-dark flex-grow-1 text-start">
            {title}
          </a>
          <button onClick={() => onDelete(id)} className="btn btn-danger btn-sm ms-3">Eliminar</button>
        </li>
      );
    }

    export default LinkItem;
    ```
    *   Fíjate que usamos la desestructuración de props `({ id, title, ... })` para hacer el código más limpio.

2.  **Refactorizar el Componente Contenedor `LinkList.jsx`:**
    *   Ahora, `LinkList.jsx` se simplifica. Su única responsabilidad es iterar sobre la lista de enlaces y renderizar un componente `LinkItem` por cada uno, pasándole las props necesarias.
    *   Importa `LinkItem` en `LinkList.jsx`.

    ```jsx
    // src/components/LinkList.jsx
    import LinkItem from './LinkItem';

    function LinkList({ links, onDeleteLink }) { // Desestructuramos las props aquí también
      return (
        <section>
          <ul className="list-unstyled d-grid gap-3">
            {links.map(link => (
              <LinkItem 
                key={link.id}
                id={link.id} // Pasamos el id para la key y para la función de borrado
                title={link.title}
                url={link.url}
                onDelete={onDeleteLink} // Pasamos la función de borrado
              />
            ))}
          </ul>
        </section>
      );
    }

    export default LinkList;
    ```

3.  **Verificar que Todo Sigue Funcionando:**
    *   Abre la aplicación en el navegador. Visualmente, nada debería haber cambiado. La funcionalidad de añadir y eliminar enlaces debe seguir intacta.
    *   Sin embargo, tu base de código ahora es más limpia, organizada y reutilizable. El componente `LinkItem` podría ser usado en cualquier otra parte de la aplicación que necesite mostrar un enlace de esta manera, sin estar atado a la lógica de la lista completa.

Este patrón te ayuda a pensar más claramente sobre las responsabilidades de tus componentes. Los componentes de presentación son fáciles de desarrollar y probar de forma aislada, mientras que los contenedores gestionan la lógica y el estado, conectando tu UI a los datos de la aplicación.
# Día 12: Construyendo la Página de Admin

## Objetivo del Día

Darle cuerpo a nuestra nueva página de administración. Además de añadir enlaces, implementaremos la funcionalidad para **eliminar** un enlace existente.

## Concepto Teórico

Para eliminar un item de una lista en el estado, necesitamos saber su `id`. Crearemos una función `deleteLink(id)` en `App.jsx`. Esta función usará el método `.filter()` de los arrays para crear una *nueva* lista que contenga todos los enlaces *excepto* el que tiene el `id` que queremos eliminar.

Recordemos la **inmutabilidad**: nunca modificamos el estado directamente. `filter` es perfecto porque devuelve un nuevo array, que es lo que le pasamos a nuestra función `setLinks`.

```javascript
const numeros = [1, 2, 3, 4];
const sinElTres = numeros.filter(numero => numero !== 3);
// sinElTres ahora es [1, 2, 4]
```

## Dibujo Conceptual

El flujo para eliminar es similar al de añadir: una función se pasa como prop desde el padre (`App`) al hijo que la necesita (`AdminPage`).

```
      +-------------------------------------------------+
      |                      App.jsx                    |
      |                                                 |
      |   Estado: `links`                               |
      |   Función: `deleteLink(id)` (filtra `links`)    |
      |                                                 |
      |       +-------------------------------------+   |
      |       |             AdminPage               |   |
      |       | props: `links`, `onDelete={deleteLink}` | --+
      |       +-------------------------------------+   | |
      +-------------------------------------------------+ |
                                                        |
      Dentro de AdminPage, cada enlace tendrá un botón "Eliminar".
      -> onClick={() => props.onDelete(link.id)}
      -> Llama a `deleteLink(link.id)` en App.jsx
      -> Actualiza el estado `links`.
```

## Tarea

1.  Crear la función `deleteLink` en `App.jsx`.
2.  Construir la UI de `AdminPage.jsx` para mostrar la lista de enlaces con un botón de eliminar para cada uno.
3.  Pasar la función `deleteLink` a `AdminPage`.

### Paso a Paso

1.  **Añadir `deleteLink` en `App.jsx`:**
    *   Define la función que recibe un `id` y actualiza el estado.
    *   Pásala como prop a `AdminPage`.

    ```jsx
    // src/App.jsx
    // ...

    function App() {
      // ... estados y addLink ...

      const deleteLink = (idToDelete) => {
        const updatedLinks = links.filter(link => link.id !== idToDelete);
        setLinks(updatedLinks);
        console.log(`Enlace con id ${idToDelete} eliminado.`);
      };

      return (
        <div className="App">
          {/* ... nav y Routes ... */}
          <Route path="/admin" element={
            <AdminPage
              links={links}
              onNewLink={addLink}
              onDeleteLink={deleteLink} // <-- Pasar la nueva función
            />
          } />
          {/* ... */}
        </div>
      );
    }
    ```

2.  **Construir `AdminPage.jsx`:**
    *   Esta página recibirá la lista de `links`, la función para añadir y la función para borrar.
    *   Mostrará el formulario y, debajo, la lista de enlaces actuales.
    *   Cada enlace en la lista tendrá un botón "Eliminar".

    ```jsx
    // src/pages/AdminPage.jsx
    import { AddLinkForm } from '../components';

    export function AdminPage({ links, onNewLink, onDeleteLink }) {
      return (
        <div>
          <h2>Administrar Enlaces</h2>
          <AddLinkForm onNewLink={onNewLink} />
          <hr />
          <h3>Mis Enlaces Actuales:</h3>
          <ul>
            {links.map(link => (
              <li key={link.id}>
                {link.title} ({link.url})
                <button onClick={() => onDeleteLink(link.id)} style={{ marginLeft: '10px' }}>
                  Eliminar
                </button>
              </li>
            ))}
          </ul>
        </div>
      );
    }
    ```
    *   **Importante:** Nota el `onClick={() => onDeleteLink(link.id)}`. Usamos una función de flecha para poder pasar el `id` del enlace específico a la función `onDeleteLink` cuando se hace clic. Si solo pusiéramos `onClick={onDeleteLink(link.id)}`, la función se ejecutaría en cada renderizado, ¡borrando todo!

### Verifica el Resultado

Ve a la página `/admin`. Deberías ver tu formulario y la lista de enlaces. Prueba a eliminar uno. Debería desaparecer de la lista. Si vuelves a la página de inicio, el enlace eliminado tampoco estará allí. ¡Ahora tienes un CRUD casi completo (Create, Read, Delete)!

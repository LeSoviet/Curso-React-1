# Día 8: Levantando el Estado (Lifting State Up)

## Objetivo del Día

Conectar nuestro `AddLinkForm` con la lista de enlaces en `App.jsx`. Para ello, aprenderás uno de los patrones más importantes de React: "levantar el estado".

## Concepto Teórico

Tenemos un problema:
-   El estado con la lista de enlaces (`links`) vive en `App.jsx`.
-   El formulario que crea un nuevo enlace (`AddLinkForm`) necesita *modificar* esa lista.

Un componente hijo no puede pasar datos directamente a un padre. La solución es que el componente padre (`App`) le pase una *función* al hijo (`AddLinkForm`) a través de las props. El hijo podrá llamar a esa función cuando lo necesite, y esa función, que fue definida en el padre, sí podrá modificar el estado del padre.

Este patrón se llama **levantar el estado** porque el estado que maneja la lógica (la lista de enlaces) se mantiene en el ancestro común más cercano (`App.jsx`).

## Dibujo Conceptual

El padre pasa una "palanca de control" (una función) al hijo.

```
      +---------------------------------------------+
      |                  App.jsx                    |
      |                                             |
      |   Estado: `links`                           |
      |   Función: `addLink(newLink)` (modifica `links`) |
      |                                             |
      |       +---------------------------------+   |
      |       |         LinkList                |   |
      |       |   props: `links`                |   |
      |       +---------------------------------+   |
      |                                             |
      |       +---------------------------------+   |
      |       |         AddLinkForm             |   |
      |       |   props: `onLinkAdd={addLink}`  | --+-- Pasa la función como prop
      |       +---------------------------------+   |   |
      +---------------------------------------------+   |
                                                        |
      Dentro de AddLinkForm, cuando el usuario envía el form:
      -> llama a `props.onLinkAdd(nuevoEnlace)`
      -> lo que en realidad ejecuta `addLink(nuevoEnlace)` en App.jsx
      -> y actualiza el estado `links`.
```

## Tarea

Modificar `App.jsx` y `AddLinkForm.jsx` para que el formulario realmente añada enlaces a la lista.

### Paso a Paso

1.  **Modificar `App.jsx`:**
    *   Crea la función `addLink` que recibe un nuevo enlace y actualiza el estado `links`.
    *   Pasa esta función como una prop (por ejemplo, `onNewLink`) a `AddLinkForm`.

    ```jsx
    // src/App.jsx
    // ...

    function App() {
      const [links, setLinks] = useState([ ... ]);
      // ...

      // 1. La función que modifica el estado padre
      const addLink = (newLinkData) => {
        const newLink = {
          id: Date.now(), // ID único
          ...newLinkData
        };
        setLinks([...links, newLink]);
      };

      return (
        <div className="App">
          {/* ... */}
          <LinkList links={links} />
          <hr />
          {/* 2. Pasa la función como prop */}
          <AddLinkForm onNewLink={addLink} />
        </div>
      );
    }
    ```

2.  **Modificar `AddLinkForm.jsx`:**
    *   El componente ahora recibe la prop `onNewLink`.
    *   En `handleSubmit`, en lugar de hacer `console.log`, llama a `onNewLink` con los datos del formulario.

    ```jsx
    // src/components/AddLinkForm.jsx
    import { useState } from 'react';

    // 1. Recibe la función en las props
    function AddLinkForm({ onNewLink }) {
      const [title, setTitle] = useState('');
      const [url, setUrl] = useState('');

      const handleSubmit = (event) => {
        event.preventDefault();
        if (!title || !url) return; // Validación simple

        // 2. Llama a la función del padre con los datos
        onNewLink({ title, url });

        setTitle('');
        setUrl('');
      };

      return (
        <form onSubmit={handleSubmit}>
          {/* ... inputs ... */}
        </form>
      );
    }

    export default AddLinkForm;
    ```

3.  **Verifica el Resultado:**
    *   Usa el formulario para añadir un nuevo enlace.
    *   ¡Debería aparecer instantáneamente en tu lista!

Has implementado uno de los patrones más cruciales de React. Entender esto te abre las puertas a crear aplicaciones complejas y bien estructuradas.

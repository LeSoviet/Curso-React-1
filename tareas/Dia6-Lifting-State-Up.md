# Día 6: Composición de Componentes y "Lifting State Up"

## Objetivo del Día

Hoy te enfocarás en la organización del código. A medida que una aplicación crece, es crucial mantener los componentes pequeños y enfocados en una sola tarea. Extraerás el formulario que creamos ayer a su propio componente (`AddLinkForm`).

Este proceso te enseñará un concepto vital: **"Lifting State Up"** (Elevar el estado). Ocurre cuando varios componentes necesitan reflejar los mismos datos cambiantes. Se logra moviendo el estado al ancestro común más cercano de los componentes que lo necesitan.

## Tarea

1.  Crear un nuevo componente `AddLinkForm.jsx`.
2.  Mover toda la lógica del formulario (los estados de los inputs y el JSX del form) de `App.jsx` al nuevo componente.
3.  Hacer que el componente `App.jsx` siga controlando la lista de enlaces, pasando la función para añadir un enlace como `prop` a `AddLinkForm`.

### Paso a Paso

1.  **Crear el Componente `AddLinkForm`:**
    *   En `src/components`, crea un nuevo archivo `AddLinkForm.jsx`.
    *   Importa `useState` en este nuevo archivo.
    *   Copia la lógica de los estados `newTitle` y `newUrl`, y el JSX del `<form>` desde `App.jsx` a `AddLinkForm.jsx`.

2.  **Modificar `AddLinkForm` para Recibir una Prop:**
    *   El componente `AddLinkForm` necesita una forma de "decirle" a `App.jsx` que un nuevo enlace debe ser añadido. Lo hará llamando a una función que recibirá a través de sus `props`.
    *   La función `handleSubmit` (antes `handleAddNewLink`) ahora llamará a `props.onNewLink` con los datos del nuevo enlace.

    ```jsx
    // src/components/AddLinkForm.jsx
    import { useState } from 'react';

    function AddLinkForm(props) {
      const [newTitle, setNewTitle] = useState('');
      const [newUrl, setNewUrl] = useState('');

      const handleSubmit = (e) => {
        e.preventDefault();
        if (!newTitle || !newUrl) return;

        // Llama a la función pasada por props con los datos
        props.onNewLink({ title: newTitle, url: newUrl });

        setNewTitle('');
        setNewUrl('');
      };

      return (
        <form onSubmit={handleSubmit}>
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
      );
    }

    export default AddLinkForm;
    ```

3.  **Limpiar y Actualizar `App.jsx`:**
    *   Elimina los estados `newTitle` y `newUrl` de `App.jsx`. Ya no los necesita.
    *   Importa tu nuevo componente `AddLinkForm`.
    *   Crea una función `addLink` en `App.jsx` que tome un objeto de enlace y actualice el estado `links`.
    *   En el `return`, renderiza `<AddLinkForm />` y pásale la función `addLink` como una prop llamada `onNewLink`.

    ```jsx
    // src/App.jsx
    import { useState } from 'react';
    // ... otras importaciones
    import AddLinkForm from './components/AddLinkForm';

    function App() {
      // ... estado de links y datos de perfil ...
      const [links, setLinks] = useState(/*...initialLinks...*/);

      const addLink = (linkData) => {
        const newLink = {
          id: Date.now(),
          ...linkData // title y url vienen de linkData
        };
        setLinks([...links, newLink]);
      };

      return (
        <div className="App">
          {/* ... ProfileHeader ... */}
          <AddLinkForm onNewLink={addLink} />
          <LinkList links={links} />
        </div>
      );
    }

    export default App;
    ```

4.  **Verificar el Flujo:**
    *   La aplicación debería funcionar exactamente igual que antes.
    *   **El flujo ahora es:**
        1.  Escribes en los inputs de `AddLinkForm`, que maneja su propio estado temporal para los campos.
        2.  Al enviar, `AddLinkForm` llama a la función `onNewLink` (que es `addLink` en `App`).
        3.  La función `addLink` en `App.jsx` recibe los datos y actualiza el estado principal `links`.
        4.  React renderiza de nuevo `App`, que pasa la nueva lista de `links` a `LinkList`, y la UI se actualiza.

Has refactorizado tu aplicación y aprendido a comunicar componentes hijos con sus padres. Esta es una habilidad clave para construir aplicaciones complejas y mantenibles.
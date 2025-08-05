# Día 7: Formularios y Componentes Controlados

## Objetivo del Día

Permitir al usuario añadir nuevos enlaces a través de un formulario. Aprenderás el concepto de "componentes controlados", donde el estado de React es la única fuente de verdad para los inputs de un formulario.

## Concepto Teórico

En HTML, los elementos de formulario (`<input>`, `<textarea>`, etc.) mantienen su propio estado interno. En React, podemos hacer que el estado del componente sea el que controle el valor del input. Esto se llama un **componente controlado**.

El flujo es así:
1.  Guardamos el valor del input en el estado de React (`useState`).
2.  El `value` del input se asigna desde ese estado.
3.  Cuando el usuario escribe, el evento `onChange` se dispara.
4.  En la función `onChange`, actualizamos el estado con el nuevo valor (`e.target.value`).
5.  Como el estado cambia, el componente se re-renderiza y el input muestra el nuevo valor del estado.

Puede parecer un círculo, pero asegura que React siempre sabe cuál es el valor del input.

## Dibujo Conceptual

El estado de React y el input del formulario están sincronizados en un ciclo.

```
      +----------------------+
      |     Estado (valor)   |
      +----------+-----------+
                 | (2. Asigna value)
                 v
      +----------------------+
      | <input value={valor}>|
      +----------+-----------+
                 ^
                 | (3. Usuario escribe, dispara onChange)
                 |
      +----------+-----------+
      |  fn onChange(e)      |
      |  setValor(e.target.value) | (4. Actualiza estado)
      +----------------------+
```

## Tarea

Crear un nuevo componente `AddLinkForm.jsx` que contenga un formulario para añadir un nuevo enlace.

### Paso a Paso

1.  **Crear `AddLinkForm.jsx`:**
    *   Crea el archivo `src/components/AddLinkForm.jsx`.
    *   Este componente necesitará su propio estado para manejar los valores de los inputs (título y URL).

    ```jsx
    // src/components/AddLinkForm.jsx
    import { useState } from 'react';

    function AddLinkForm() {
      const [title, setTitle] = useState('');
      const [url, setUrl] = useState('');

      const handleSubmit = (event) => {
        event.preventDefault(); // Evita que la página se recargue
        console.log('Nuevo enlace:', { title, url });
        // Lógica para añadir el enlace (Día 8)
        setTitle(''); // Limpiar input
        setUrl('');   // Limpiar input
      };

      return (
        <form onSubmit={handleSubmit}>
          <input
            type="text"
            placeholder="Título del enlace"
            value={title}
            onChange={(e) => setTitle(e.target.value)}
          />
          <input
            type="text"
            placeholder="URL"
            value={url}
            onChange={(e) => setUrl(e.target.value)}
          />
          <button type="submit">Añadir</button>
        </form>
      );
    }

    export default AddLinkForm;
    ```

2.  **Integrar en `App.jsx`:**
    *   Importa `AddLinkForm` en `App.jsx`.
    *   Colócalo donde quieras que aparezca el formulario (por ejemplo, después de la lista de enlaces).

    ```jsx
    // src/App.jsx
    // ... imports
    import AddLinkForm from './components/AddLinkForm'; // <-- Importar

    function App() {
      // ... estados y funciones
      return (
        <div className="App">
          {/* ... */}
          <LinkList links={links} />
          <hr />
          <AddLinkForm />
        </div>
      );
    }

    export default App;
    ```

3.  **Verifica el Resultado:**
    *   Ahora deberías ver un formulario en tu página.
    *   Intenta escribir en los inputs.
    *   Cuando presiones "Añadir", revisa la consola. Verás el objeto del nuevo enlace. Los campos del formulario se limpiarán.

El formulario aún no *añade* el enlace a la lista. ¡Ese es el desafío del próximo día!

# Día 4: Manejando Eventos con `onClick`

## Objetivo del Día

Aprender a responder a las interacciones del usuario, como hacer clic en un botón. Añadiremos un botón que, en el futuro, nos permitirá agregar nuevos enlaces a nuestra lista.

## Concepto Teórico

React nos permite adjuntar "manejadores de eventos" a los elementos JSX. Estos son muy parecidos a los del HTML (`onclick`, `onchange`), pero en React se escriben en `camelCase` (ej: `onClick`, `onChange`).

Un manejador de evento es simplemente una función que se ejecuta cuando ocurre un evento específico.

```jsx
<button onClick={miFuncion}>Haz Clic</button>
```

Cuando el usuario hace clic en el botón, la `miFuncion` se ejecutará.

## Dibujo Conceptual

El flujo de un evento es simple: el usuario actúa, React lo detecta y ejecuta tu código.

```
  +--------------+      +---------------------+      +----------------------+
  |              |      |                     |      |                      |
  | Usuario hace |----->|   Elemento JSX      |----->|  Función Manejadora  |
  |    clic      |      | <button onClick={fn}> |      |       (tu código)    |
  |              |      |                     |      |                      |
  +--------------+      +---------------------+      +----------------------+
```

## Tarea

Añadiremos un botón a nuestra aplicación. Al hacer clic, mostraremos un mensaje en la consola del navegador. También crearemos la función que en el futuro añadirá un nuevo enlace.

### Paso a Paso

1.  **Modificar `App.jsx`:**
    *   Dentro del componente `App`, define una nueva función llamada `handleAddLink`.
    *   Por ahora, esta función solo imprimirá un mensaje en la consola.
    *   Añade un botón en tu JSX y asígnale la función `handleAddLink` al evento `onClick`.

    ```jsx
    // src/App.jsx
    import { useState } from 'react';
    import './App.css';
    // ... otros imports

    function App() {
      const profileData = { ... };
      const [links, setLinks] = useState([ ... ]); // Estado del día anterior

      // 1. Define la función manejadora
      const handleAddLink = () => {
        // Por ahora, solo un console.log
        console.log('¡Botón presionado! Próximamente añadiremos un enlace.');
        
        // En el futuro, aquí actualizaremos el estado:
        const newLink = { id: Date.now(), title: 'Nuevo Enlace', url: '#' };
        // setLinks([...links, newLink]); // <-- Esto lo activaremos después
      };

      return (
        <div className="App">
          <ProfileHeader ... />
          <LinkList links={links} />

          {/* 2. Añade el botón con el evento onClick */}
          <button onClick={handleAddLink}>
            Añadir Nuevo Enlace
          </button>
        </div>
      );
    }

    export default App;
    ```

2.  **Verifica el Resultado:**
    *   Abre tu aplicación en el navegador.
    *   Abre las herramientas de desarrollador (usualmente con F12) y ve a la pestaña "Consola".
    *   Haz clic en el nuevo botón "Añadir Nuevo Enlace".
    *   Deberías ver el mensaje "¡Botón presionado! Próximamente añadiremos un enlace." en la consola.

¡Excelente! Has conectado una acción del usuario con la lógica de tu aplicación. Este es un pilar fundamental de la interactividad en la web.

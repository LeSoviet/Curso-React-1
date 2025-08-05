# Día 3: Estado Dinámico con `useState`

## Objetivo del Día

Aprender a manejar datos que cambian en tu aplicación utilizando el hook `useState`. Esto nos permitirá tener una lista de enlaces dinámica en lugar de enlaces fijos en el código.

## Concepto Teórico

En React, el "estado" es un objeto que guarda datos que pueden cambiar con el tiempo. Cuando el estado cambia, React automáticamente vuelve a renderizar el componente para mostrar los datos actualizados. El hook `useState` es la herramienta que nos permite añadir estado a nuestros componentes funcionales.

`const [valor, setValor] = useState(valorInicial);`

- `valor`: La variable que contiene el valor actual del estado.
- `setValor`: La función que usamos para actualizar el `valor`.
- `valorInicial`: El valor que tendrá el estado la primera vez que el componente se renderiza.

## Dibujo Conceptual

Imagina que tu componente tiene una "memoria" interna. `useState` nos da acceso a ella.

```
      +-------------------------+
      |      COMPONENTE         |
      |                         |
      |   +-----------------+   |
      |   |   estado (links)  |   |  <-- `useState` crea esta "caja" de memoria
      |   +-----------------+   |
      |           |             |
      |           v             |
      |      Renderiza la UI    |
      |      basada en `links`  |
      +-------------------------+
```

## Tarea

Vamos a refactorizar `App.jsx` y `LinkList.jsx` para usar `useState` y manejar una lista de enlaces.

### Paso a Paso

1.  **Modificar `App.jsx`:**
    *   Importa `useState` desde React: `import { useState } from 'react';`
    *   Dentro del componente `App`, antes del `return`, crea un estado para guardar los enlaces.

    ```jsx
    // src/App.jsx
    import { useState } from 'react'; // <-- Importar
    import './App.css';
    import ProfileHeader from './components/ProfileHeader';
    import LinkList from './components/LinkList';

    function App() {
      const profileData = { ... }; // Mantén esto igual

      // Crear el estado para los enlaces
      const [links, setLinks] = useState([
        { id: 1, title: 'Mi Portafolio', url: 'https://github.com' },
        { id: 2, title: 'Mi LinkedIn', url: 'https://linkedin.com' },
        { id: 3, title: 'Mi Twitter', url: 'https://twitter.com' }
      ]);

      return (
        <div className="App">
          <ProfileHeader ... />
          {/* Pasa la lista de enlaces al componente LinkList */}
          <LinkList links={links} />
        </div>
      );
    }

    export default App;
    ```

2.  **Modificar `LinkList.jsx`:**
    *   El componente ahora recibirá una prop `links`.
    *   En lugar de tener una lista fija, usaremos la lista que llega por `props`. Por ahora, solo mostraremos la cantidad de enlaces para verificar que funciona.

    ```jsx
    // src/components/LinkList.jsx

    // El componente ahora espera recibir 'links' como prop
    function LinkList({ links }) {
      return (
        <section>
          <p>Tienes {links.length} enlaces.</p>
          {/* En el Día 5, renderizaremos la lista completa aquí */}
        </section>
      );
    }

    export default LinkList;
    ```

3.  **Verifica el Resultado:**
    *   Ve a tu navegador. Deberías ver el mensaje: "Tienes 3 enlaces."
    *   ¡Felicidades! Has creado tu primer estado. Ahora los datos de tu aplicación son dinámicos.

En el próximo día, haremos que esta lista se muestre correctamente.

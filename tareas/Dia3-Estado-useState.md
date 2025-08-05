# Día 3: Introducción al Estado con `useState`

## Objetivos del Día

- Entender qué es el "estado" en React.
- Aprender a usar el Hook `useState` para manejar datos que cambian.
- Mover nuestro array de enlaces al estado del componente.

## Tareas

### 1. Entendiendo el Estado

**¿Qué es el "Estado"?**

Imagina que tienes una variable en tu componente, como un contador. Cuando el valor de esa variable cambia (por ejemplo, al hacer clic en un botón), quieres que React actualice la interfaz de usuario para mostrar el nuevo valor. 

Si usaras una variable normal de JavaScript, React no se enteraría del cambio y la pantalla no se actualizaría. 

Aquí es donde entra el **estado**. El estado es una memoria que React le da a cada componente. Cuando cambias una variable de estado, React lo sabe y automáticamente vuelve a renderizar el componente y sus hijos, actualizando la UI con los nuevos datos. 

Para manejar el estado en componentes de función, usamos los **Hooks**. El primer Hook que aprenderemos es `useState`.

### 2. Importar `useState`

**Paso a paso:**

1.  Abre el archivo `src/App.jsx`.
2.  En la primera línea, donde importas `React`, modifica la línea para importar también `useState`.

**Antes:**

```jsx
import React from 'react';
```

**Después:**

```jsx
import { useState } from 'react';
```

**¿Por qué y qué hace?**

*   **`import { useState } ...`**: Estamos importando la función `useState` desde la biblioteca de React. Las llaves `{}` indican que estamos haciendo una "importación nombrada", es decir, estamos importando una parte específica de la biblioteca de React, no la biblioteca entera.
*   **`useState`** es un **Hook**. Los Hooks son funciones especiales que te permiten "engancharte" a las características de React (como el estado y el ciclo de vida) desde tus componentes de función.

### 3. Mover los enlaces al estado

**Paso a paso:**

1.  Dentro de la función `App`, localiza la línea donde definiste el array `links`.
2.  Vamos a reemplazar esa declaración con una llamada a `useState`.

**Antes:**

```jsx
function App() {
  const links = [
    { id: 1, title: 'Mi Portafolio', url: 'https://github.com/johndoe' },
    // ...etc
  ];
  // ...
}
```

**Después:**

```jsx
function App() {
  const [links, setLinks] = useState([
    { id: 1, title: 'Mi Portafolio', url: 'https://github.com/johndoe' },
    { id: 2, title: 'Twitter', url: 'https://twitter.com/johndoe' },
    { id: 3, title: 'LinkedIn', url: 'https://linkedin.com/in/johndoe' },
    { id: 4, title: 'Instagram', url: 'https://instagram.com/johndoe' }
  ]);

  // ... el resto del componente
}
```

**¿Por qué y qué hace?**

*   **`useState(...)`**: Estamos llamando al Hook `useState`. El argumento que le pasamos (`[...]`) es el **valor inicial** de nuestra variable de estado. En este caso, nuestro array de enlaces.

*   **`const [links, setLinks] = ...`**: Esto se llama "desestructuración de array". Es una sintaxis de JavaScript que nos permite desempacar valores de un array en distintas variables. El Hook `useState` siempre devuelve un array con exactamente dos elementos:
    1.  **La variable de estado actual (`links`)**: Esta es la variable que contiene el valor actual de nuestro estado (el array de enlaces). La usaremos en nuestro JSX para renderizar la lista, igual que antes.
    2.  **La función de actualización (`setLinks`)**: Esta es la función que debemos usar para **actualizar** el estado. **Nunca debes modificar la variable de estado directamente** (por ejemplo, haciendo `links.push(...)`). Siempre debes usar la función de actualización (`setLinks(...)`) para que React sepa que el estado ha cambiado y que necesita volver a renderizar el componente.

**¿Por qué mover los datos al estado?**

Por ahora, la aplicación se verá y funcionará exactamente igual. Sin embargo, hemos dado un paso crucial. Al tener nuestros enlaces en el estado, hemos preparado nuestra aplicación para ser **interactiva**. 

En los próximos días, querremos hacer cosas como añadir un nuevo enlace a través de un formulario o eliminar un enlace existente. Para que la UI se actualice cuando realicemos estas acciones, los datos **deben** estar en el estado. Cuando llamemos a `setLinks` con un nuevo array (por ejemplo, un array con un enlace más), React se encargará de volver a renderizar la lista de `LinkCard` para reflejar ese cambio. Si los datos fueran una simple constante, no podríamos cambiarlos y la UI sería estática.
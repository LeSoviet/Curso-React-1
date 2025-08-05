# Día 10: Refactor y Estructura del Proyecto

## Objetivo del Día

A medida que un proyecto crece, mantener el código organizado es crucial. Hoy no aprenderemos un concepto nuevo de React, sino una habilidad esencial de ingeniería de software: la refactorización y la organización de archivos.

## Concepto Teórico

**Refactorizar** significa cambiar la estructura interna de un código sin cambiar su comportamiento externo. El objetivo es mejorar su legibilidad, mantenibilidad y reducir su complejidad.

**Estructura de Archivos:** No hay una única forma "correcta" de organizar un proyecto de React, pero hay patrones comunes. Uno muy popular es agrupar los archivos por "feature" o por tipo. Empezaremos con una estructura simple por tipo:

```
src/
|-- components/       # Componentes de UI reutilizables
|   |-- ProfileHeader.jsx
|   |-- LinkList.jsx
|   |-- LinkCard.jsx
|   |-- AddLinkForm.jsx
|   |-- index.js        # (Opcional) Para exportaciones más limpias
|
|-- assets/           # Imágenes, SVGs, fuentes, etc.
|   |-- logo.svg
|
|-- App.jsx           # Componente principal de la aplicación
|-- App.css           # Estilos principales
|-- main.jsx          # Punto de entrada de la aplicación
```

## Tarea

1.  Crear un archivo "barrel" (`index.js`) para simplificar las importaciones de componentes.
2.  Mover la data inicial a un archivo separado.
3.  Limpiar y organizar `App.jsx`.

### Paso a Paso

1.  **Crear Archivo "Barrel" en `components`:**
    *   Dentro de `src/components`, crea un nuevo archivo `index.js`.
    *   Este archivo importará todos los componentes de la carpeta y los exportará en un solo lugar.

    ```javascript
    // src/components/index.js
    export { default as ProfileHeader } from './ProfileHeader';
    export { default as LinkList } from './LinkList';
    export { default as LinkCard } from './LinkCard';
    export { default as AddLinkForm } from './AddLinkForm';
    ```

2.  **Actualizar Importaciones en `App.jsx`:**
    *   Ahora puedes importar todos los componentes en una sola línea.

    ```jsx
    // src/App.jsx
    // Antes:
    // import ProfileHeader from './components/ProfileHeader';
    // import LinkList from './components/LinkList';
    // import AddLinkForm from './components/AddLinkForm';

    // Ahora:
    import { ProfileHeader, LinkList, AddLinkForm } from './components';
    ```

3.  **Mover Datos Iniciales:**
    *   Crea una nueva carpeta `src/data`.
    *   Dentro, crea un archivo `initialData.js`.
    *   Mueve los objetos de `profileData` y `links` iniciales a este archivo.

    ```javascript
    // src/data/initialData.js
    export const initialProfileData = {
      imageUrl: 'https://via.placeholder.com/150',
      username: '@tu-usuario-pro',
      bio: 'Desarrollador Frontend en formación continua.'
    };

    export const initialLinks = [
      { id: 1, title: 'Mi Portafolio', url: 'https://github.com' },
      { id: 2, title: 'Mi LinkedIn', url: 'https://linkedin.com' }
    ];
    ```

4.  **Limpiar `App.jsx`:**
    *   Importa los datos iniciales y úsalos en tus `useState`.

    ```jsx
    // src/App.jsx
    import { useState, useEffect } from 'react';
    import { ProfileHeader, LinkList, AddLinkForm } from './components';
    import { initialProfileData, initialLinks } from './data/initialData'; // <--
    import './App.css';

    function App() {
      const [profile, setProfile] = useState(initialProfileData);
      const [links, setLinks] = useState(initialLinks);

      // ... el resto de la lógica (useEffect, addLink) ...
      // Usa `profile.username` en lugar de `profileData.username`

      return (
        <div className="App">
          <ProfileHeader
            imageUrl={profile.imageUrl}
            username={profile.username}
            bio={profile.bio}
          />
          {/* ... */}
        </div>
      );
    }
    ```

### Verifica el Resultado

Una vez más, la aplicación debería funcionar exactamente igual. Pero tu código base ahora está mucho más limpio, es más fácil de navegar y está preparado para crecer. Has invertido tiempo en la calidad de tu código, una marca de un desarrollador profesional.

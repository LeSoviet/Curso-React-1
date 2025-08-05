# Día 8: Estilizando la Aplicación con Bootstrap

## Objetivos del Día

-   Instalar una librería de CSS externa (Bootstrap) en nuestro proyecto.
-   Integrar las clases de Bootstrap para mejorar el diseño de la aplicación.
-   Entender cómo usar un sistema de rejilla (Grid) básico para el layout.

## Tareas

### 1. ¿Qué es Bootstrap?

Bootstrap es un framework de CSS muy popular. Nos proporciona un conjunto de clases de CSS predefinidas para estilizar componentes comunes (botones, formularios, tarjetas, etc.) y un sistema de rejilla (Grid) para crear layouts responsivos (que se adaptan a diferentes tamaños de pantalla) de forma rápida y sencilla.

En lugar de escribir todo nuestro CSS desde cero, podemos aprovechar estas clases para darle a nuestra aplicación un aspecto profesional con muy poco esfuerzo.

### 2. Instalar Bootstrap

Vamos a añadir Bootstrap a nuestro proyecto como una dependencia.

**Paso a paso:**

1.  Abre la terminal de tu editor de código (normalmente puedes ir a `Terminal > Nueva Terminal` en el menú superior).
2.  Asegúrate de que estás en la carpeta raíz de tu proyecto (`linkhub-project`).
3.  Ejecuta el siguiente comando:

    ```bash
    npm install bootstrap
    ```

**¿Por qué y qué hace?**

*   **`npm install`**: `npm` es el gestor de paquetes de Node.js. Este comando se usa para instalar paquetes (librerías) de terceros en nuestro proyecto.
*   **`bootstrap`**: Es el nombre del paquete que queremos instalar.
*   Al ejecutar esto, `npm` descargará los archivos de Bootstrap en tu carpeta `node_modules` y añadirá `bootstrap` como una dependencia en tu archivo `package.json`, para que el proyecto sepa que necesita esta librería para funcionar.

### 3. Importar el CSS de Bootstrap

Para que las clases de Bootstrap estén disponibles en nuestra aplicación, necesitamos importar su archivo CSS principal.

**Paso a paso:**

1.  Abre el archivo `src/main.jsx`. Este es el punto de entrada principal de tu aplicación, por lo que es el mejor lugar para importar estilos globales.
2.  Añade la siguiente línea de importación en la parte superior del archivo.

**Código a añadir en `src/main.jsx`:**

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App.jsx';
import './index.css';

// Importar el CSS de Bootstrap
import 'bootstrap/dist/css/bootstrap.min.css';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```

**¿Por qué y qué hace?**

*   **`import 'bootstrap/dist/css/bootstrap.min.css';`**: Esta línea le dice a Vite (nuestro empaquetador de proyectos) que encuentre el archivo CSS de Bootstrap dentro de la carpeta `node_modules` y lo incluya en el paquete final de nuestra aplicación. El archivo `.min.css` es una versión "minificada" (comprimida) del CSS, optimizada para la producción.
*   Al importarlo en `main.jsx`, nos aseguramos de que estos estilos se carguen primero y estén disponibles para todos los componentes de la aplicación.

### 4. Aplicar Clases de Bootstrap

Ahora viene la parte divertida. Vamos a recorrer nuestros componentes y reemplazar nuestros `className` personalizados (o añadir nuevos) con las clases de Bootstrap.

**Paso a paso:**

1.  **En `App.jsx`:** Envuelve todo en un contenedor de Bootstrap para centrar el contenido.

    ```jsx
    // En App.jsx
    return (
      <div className="container mt-5"> // Contenedor principal con margen superior
        {/* ... el resto del contenido ... */}
      </div>
    );
    ```

2.  **En `LinkCard.jsx`:** Usa las clases de Bootstrap para tarjetas y botones.

    ```jsx
    // En LinkCard.jsx
    return (
      <div className="card card-body mb-3"> // Tarjeta con padding y margen inferior
        <a href={url} target="_blank" rel="noopener noreferrer" className="d-block"> // Enlace como bloque
          {title}
        </a>
        <button onClick={handleDelete} className="btn btn-danger btn-sm position-absolute top-0 end-0 m-2"> // Botón rojo, pequeño y posicionado
          Eliminar
        </button>
      </div>
    );
    ```

3.  **En `AddLinkForm.jsx`:** Usa las clases de Bootstrap para formularios.

    ```jsx
    // En AddLinkForm.jsx
    return (
      <form onSubmit={handleSubmit} className="mb-4"> // Formulario con margen inferior
        <h2 className="mb-3">Añadir Nuevo Enlace</h2>
        <div className="mb-3"> // Grupo de formulario con margen inferior
          <input
            type="text"
            placeholder="Título del enlace"
            value={title}
            onChange={(e) => setTitle(e.target.value)}
            className="form-control" // Estilo de input de Bootstrap
          />
        </div>
        <div className="mb-3">
          <input
            type="url"
            placeholder="URL del enlace"
            value={url}
            onChange={(e) => setUrl(e.target.value)}
            className="form-control"
          />
        </div>
        <button type="submit" className="btn btn-primary">Añadir</button> // Botón azul primario
      </form>
    );
    ```

**¿Por qué y qué hace?**

*   **`container`**: Centra el contenido y le da un ancho máximo.
*   **`mt-5`, `mb-3`, `m-2`**: Clases de utilidad para márgenes (`m`argin) y `t`op, `b`ottom. El número va del 1 al 5.
*   **`card`, `card-body`**: Crean un contenedor con estilo de tarjeta.
*   **`btn`, `btn-danger`, `btn-primary`, `btn-sm`**: Estilizan botones con colores y tamaños específicos.
*   **`position-absolute`, `top-0`, `end-0`**: Clases de utilidad para posicionar elementos.
*   **`form-control`**: Le da a los inputs el estilo estándar de Bootstrap.

Al simplemente añadir estas clases, tu aplicación debería pasar de verse muy simple a tener un aspecto mucho más pulido y estructurado. Te animo a que explores la [documentación de Bootstrap](https://getbootstrap.com/docs/5.3/getting-started/introduction/) para descubrir la enorme cantidad de clases y componentes que puedes usar.

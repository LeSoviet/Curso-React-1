# Día 15: Toques Finales y Preparación para Despliegue

## Objetivo del Día

¡Llegaste al final! Hoy nos dedicaremos a pulir la aplicación, añadir algunos toques finales de calidad y aprender cómo preparar nuestro proyecto para ser subido a un servidor web (despliegue).

## Conceptos Teóricos

**1. `Prop-Types`:**
Es una buena práctica (especialmente en proyectos de JavaScript, no tanto en TypeScript) documentar y validar las `props` que reciben tus componentes. La librería `prop-types` nos ayuda a definir el tipo esperado para cada prop (string, number, array, etc.). Si un componente recibe una prop de un tipo incorrecto durante el desarrollo, veremos un aviso en la consola.

**2. Build de Producción:**
Hasta ahora, hemos trabajado en un entorno de *desarrollo* (`npm run dev`). Este entorno está optimizado para la velocidad de recarga y la depuración. Para *producción*, necesitamos una versión optimizada de nuestro código: minimizada, empaquetada y lista para ser servida eficientemente. Vite se encarga de esto con el comando `npm run build`.

Este comando crea una carpeta `dist` con los archivos estáticos (HTML, CSS, JS) que componen tu aplicación. Estos son los archivos que subirías a un servicio de hosting.

## Dibujo Conceptual

El proceso de "build" transforma tu código de desarrollo en un paquete optimizado para producción.

```
      +--------------------------------+
      |      Tu Código Fuente          |
      |  (src/, components/, pages/)   |
      |  (Múltiples archivos, JSX)     |
      +---------------+----------------+
                      |
                      v (npm run build)
      +---------------+----------------+
      |      Proceso de Vite/Build     |
      | - Compila JSX a JS             |
      | - Empaqueta JS en pocos files  |
      | - Minimiza CSS y JS            |
      | - Optimiza imágenes            |
      +---------------+----------------+
                      |
                      v
      +--------------------------------+
      |         Carpeta `dist`         |
      | - index.html                   |
      | - assets/app.min.js            |
      | - assets/style.min.css         |
      +--------------------------------+
```

## Tarea

1.  Instalar y usar `prop-types` en un par de componentes.
2.  Ejecutar el build de producción.
3.  Añadir algunos estilos finales para que se vea mejor.

### Paso a Paso

1.  **Instalar `prop-types`:**
    ```bash
    npm install prop-types
    ```

2.  **Aplicar `PropTypes` en `LinkCard.jsx`:**
    ```jsx
    // src/components/LinkCard.jsx
    import PropTypes from 'prop-types';

    function LinkCard({ link }) {
      // ...
    }

    // Definimos los tipos de las props esperadas
    LinkCard.propTypes = {
      link: PropTypes.shape({
        id: PropTypes.number.isRequired,
        title: PropTypes.string.isRequired,
        url: PropTypes.string.isRequired,
      }).isRequired,
    };

    export default LinkCard;
    ```

3.  **Añadir Estilos Finales:**
    *   ¡Esto es libre! Ve a `App.css` o crea nuevos archivos CSS para tus componentes.
    *   Intenta darle un ancho máximo al contenedor principal, centrarlo, y darle un estilo más bonito a los botones y enlaces.
    *   Ejemplo en `App.css`:
    ```css
    .App {
      max-width: 680px;
      margin: 40px auto;
      padding: 20px;
    }
    /* ... más estilos ... */
    ```

4.  **Crear el Build de Producción:**
    *   En tu terminal, ejecuta:
        ```bash
        npm run build
        ```
    *   Una vez que termine, verás una nueva carpeta `dist` en la raíz de tu proyecto. Explora su contenido. Verás un `index.html` y una carpeta `assets` con archivos de nombres extraños (es el código optimizado).

### ¡Felicidades, has completado el programa de Trainee!

Has construido una aplicación de React desde cero, aprendiendo conceptos clave como componentes, props, estado, efectos, enrutamiento y comunicación entre componentes.

**¿Qué sigue?**
-   Puedes desplegar tu carpeta `dist` en servicios gratuitos como [Netlify](https://www.netlify.com/) o [Vercel](https://vercel.com/).
-   Intenta añadir más funcionalidades: ¿un formulario para editar enlaces? ¿Reordenar la lista? ¿Autenticación de usuarios?
-   Explora librerías de componentes como [Material-UI](https://mui.com/) o [Chakra UI](https://chakra-ui.com/).

Has demostrado una gran capacidad de aprendizaje. ¡Sigue así!

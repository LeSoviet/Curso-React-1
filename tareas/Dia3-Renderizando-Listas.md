# Día 3: Renderizando Listas Dinámicas

## Objetivo del Día

Hoy aprenderás a mostrar datos de una colección (un array) en tu aplicación. Este es un patrón fundamental en React, ya que rara vez mostrarás contenido estático. Usaremos el método `.map()` de JavaScript para generar una lista de componentes a partir de un array de datos.

## Tarea

Modificarás el componente `LinkList` para que muestre una lista de enlaces proveniente de un array de objetos, en lugar de los enlaces estáticos que escribimos ayer.

### Paso a Paso

1.  **Definir los Datos de los Enlaces:**
    *   En tu componente `App.jsx`, justo antes del `return`, crea un array de objetos llamado `linksData`. Cada objeto representará un enlace y tendrá propiedades como `id`, `title`, y `url`.

    ```javascript
    // src/App.jsx
    // ...dentro del componente App, antes del return

    const linksData = [
      { id: 1, title: 'Mi Portafolio', url: 'https://github.com' },
      { id: 2, title: 'Mi LinkedIn', url: 'https://linkedin.com' },
      { id: 3, title: 'Mi Twitter', url: 'https://twitter.com' },
      { id: 4, title: 'Mi Blog Personal', url: 'https://dev.to' }
    ];
    ```

2.  **Pasar los Datos como Prop:**
    *   Ahora, pasa este array `linksData` como una `prop` a tu componente `LinkList`.

    ```jsx
    // src/App.jsx
    // ...en el return de App
    <LinkList links={linksData} />
    ```

3.  **Modificar `LinkList` para Usar `.map()`:**
    *   Abre `src/components/LinkList.jsx`.
    *   El componente ahora recibe la prop `links`.
    *   Dentro del `<ul>`, usa el método `links.map()` para iterar sobre el array. Por cada `link` en el array, devuelve un elemento `<li>` con un `<a>` adentro.
    *   **Importante:** Cada elemento que generas en un `.map()` necesita una `prop` especial y única llamada `key`. Usaremos el `id` del enlace para esto. React usa la `key` para identificar qué elementos han cambiado, se han agregado o se han eliminado.

    ```jsx
    // src/components/LinkList.jsx

    function LinkList(props) {
      return (
        <section>
          <ul>
            {props.links.map(link => (
              <li key={link.id}>
                <a href={link.url} target="_blank" rel="noopener noreferrer">
                  {link.title}
                </a>
              </li>
            ))}
          </ul>
        </section>
      );
    }

    export default LinkList;
    ```
    *   Nota que hemos añadido `target="_blank"` y `rel="noopener noreferrer"` al enlace. Es una buena práctica para abrir enlaces externos en una nueva pestaña de forma segura.

4.  **Ver el Resultado:**
    *   Revisa tu navegador. Ahora deberías ver la lista de enlaces generada dinámicamente a partir de tu array de datos.

¡Excelente! Has aprendido a transformar datos en UI, una de las tareas más comunes en React.
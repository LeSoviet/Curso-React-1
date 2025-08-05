# Día 5: Renderizando Listas con `.map()`

## Objetivo del Día

Mostrar dinámicamente la lista de enlaces que tenemos en nuestro estado. Aprenderás a usar el método `.map()` de JavaScript, una técnica esencial en React para renderizar colecciones de datos.

## Concepto Teórico

No podemos simplemente poner un array de objetos en nuestro JSX. React no sabría cómo mostrarlo. La solución es "mapear" (transformar) cada elemento del array en un elemento JSX.

El método `.map()` de los arrays en JavaScript es perfecto para esto. Itera sobre cada elemento de un array y devuelve un nuevo array con los resultados de aplicar una función a cada elemento.

```javascript
const numeros = [1, 2, 3];
const duplicados = numeros.map(numero => numero * 2);
// duplicados ahora es [2, 4, 6]
```

En React, lo usamos para transformar un array de datos en un array de componentes.

**La prop `key`:** Cuando renderizas una lista, React necesita una forma de identificar cada elemento de forma única para optimizar el renderizado. Para esto, debemos pasar una prop especial llamada `key` a cada elemento raíz de la lista. La `key` debe ser un string o número único entre los hermanos de la lista. Usualmente se usa el `id` del dato.

## Dibujo Conceptual

El método `.map()` actúa como una fábrica que convierte datos crudos en componentes visuales.

```
      +---------------------+
      |   Estado (Array)    |
      | [ {id:1}, {id:2} ]  |
      +----------+----------+
                 |
                 v .map(item => <Componente key={item.id} />)
      +----------+----------+
      | Array de Componentes|
      | [ <Comp1/>, <Comp2/> ] |
      +---------------------+
                 |
                 v
      +---------------------+
      |     UI Renderizada  |
      |      - Componente 1 |
      |      - Componente 2 |
      +---------------------+
```

## Tarea

Modificar el componente `LinkList.jsx` para que muestre la lista de enlaces que recibe a través de las `props`.

### Paso a Paso

1.  **Modificar `LinkList.jsx`:**
    *   Dentro del componente, justo antes del `return`, usa el método `links.map()` para crear un `<li>` por cada enlace.
    *   Asegúrate de pasar una `key` única a cada `<li>`. Usaremos `link.id`.
    *   Dentro de cada `<li>`, pon un `<a>` que muestre el `link.title` y apunte a `link.url`.

    ```jsx
    // src/components/LinkList.jsx

    function LinkList({ links }) {
      return (
        <section>
          <ul>
            {/* Usamos .map() para transformar cada objeto 'link' en un <li> */}
            {links.map(link => (
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
    *Nota: `target="_blank"` abre el enlace en una nueva pestaña. `rel="noopener noreferrer"` es una buena práctica de seguridad para esto.*

2.  **Verifica el Resultado:**
    *   Ve a tu navegador. Ahora deberías ver la lista de enlaces que definiste en el estado de `App.jsx`.
    *   Si haces clic en uno, se abrirá la URL correspondiente en una nueva pestaña.

¡Felicidades! Has dominado una de las técnicas más comunes y poderosas de React. Ahora puedes mostrar cualquier cantidad de datos de forma dinámica.

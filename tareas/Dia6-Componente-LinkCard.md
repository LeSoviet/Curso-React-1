# Día 6: Creando el Componente `LinkCard`

## Objetivo del Día

Mejorar la modularidad de nuestro código creando un componente específico para mostrar cada enlace individual. Esto hará nuestro código más limpio, reutilizable y fácil de mantener.

## Concepto Teórico

A medida que una aplicación crece, es una buena práctica dividir la UI en componentes más pequeños y especializados. En lugar de tener toda la lógica de renderizado de un enlace dentro de `LinkList`, podemos encapsularla en su propio componente, por ejemplo, `LinkCard`.

`LinkList` se encargará de la *lista*, y `LinkCard` se encargará de *un solo item* de esa lista. Esto se llama **Composición de Componentes**.

## Dibujo Conceptual

Estamos anidando componentes. `App` contiene a `LinkList`, y `LinkList` contendrá múltiples `LinkCard`.

```
      +-----------------------------------+
      |                App                |
      |  +-----------------------------+  |
      |  |          LinkList           |  |
      |  |                             |  |
      |  |  +-----------------------+  |  |
      |  |  |  LinkCard (link 1)    |  |  |
      |  |  +-----------------------+  |  |
      |  |  +-----------------------+  |  |
      |  |  |  LinkCard (link 2)    |  |  |
      |  |  +-----------------------+  |  |
      |  |  +-----------------------+  |  |
      |  |  |  LinkCard (link 3)    |  |  |
      |  |  +-----------------------+  |  |
      |  |                             |  |
      |  +-----------------------------+  |
      +-----------------------------------+
```

## Tarea

1.  Crear un nuevo componente `LinkCard.jsx`.
2.  Usar `LinkCard` dentro de `LinkList.jsx` para renderizar cada enlace.

### Paso a Paso

1.  **Crear `LinkCard.jsx`:**
    *   En la carpeta `src/components`, crea un nuevo archivo: `LinkCard.jsx`.
    *   Este componente recibirá un solo `link` como prop.
    *   Su `return` será la estructura JSX de un enlace individual (el `<li>` y el `<a>`).

    ```jsx
    // src/components/LinkCard.jsx

    // Recibimos el objeto 'link' completo como prop
    function LinkCard({ link }) {
      return (
        <li>
          <a href={link.url} target="_blank" rel="noopener noreferrer">
            {link.title}
          </a>
        </li>
      );
    }

    export default LinkCard;
    ```

2.  **Refactorizar `LinkList.jsx`:**
    *   Importa tu nuevo componente: `import LinkCard from './LinkCard';`.
    *   Dentro del `.map()`, en lugar de devolver un `<li>`, devuelve un componente `<LinkCard />`.
    *   Pásale el `link` individual y la `key` al componente `LinkCard`.

    ```jsx
    // src/components/LinkList.jsx
    import LinkCard from './LinkCard'; // <-- Importar

    function LinkList({ links }) {
      return (
        <section>
          <ul>
            {links.map(link => (
              // Ahora usamos el componente LinkCard
              <LinkCard key={link.id} link={link} />
            ))}
          </ul>
        </section>
      );
    }

    export default LinkList;
    ```

3.  **Verifica el Resultado:**
    *   Visualmente, ¡nada debería haber cambiado!
    *   Pero internamente, tu código ahora es mucho más profesional. Has separado las responsabilidades. Si mañana necesitas cambiar cómo se ve un enlace (añadir un icono, por ejemplo), solo tienes que modificar `LinkCard.jsx`, sin tocar `LinkList.jsx`.

Este es el corazón de la filosofía de React: construir UIs complejas a partir de piezas pequeñas y reutilizables.

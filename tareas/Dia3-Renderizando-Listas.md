# Día 3: Renderizando Listas

## Objetivos del Día

- Almacenar los datos de los enlaces en un array.
- Usar el método `.map()` para renderizar dinámicamente la lista de componentes `LinkCard`.

## Tareas

### 1. Crear un array de datos para los enlaces

**Paso a paso:**

1.  Abre el archivo `src/App.jsx`.
2.  Dentro de la función `App`, pero **antes** de la declaración `return`, crea un array de objetos llamado `links`.
3.  Cada objeto en el array representará un enlace y tendrá dos propiedades: `id` y `title`, y `url`.

**Código a añadir en `App.jsx`:**

```jsx
function App() {
  // Array de datos de enlaces
  const links = [
    { id: 1, title: 'Mi Portafolio', url: 'https://github.com/johndoe' },
    { id: 2, title: 'Twitter', url: 'https://twitter.com/johndoe' },
    { id: 3, title: 'LinkedIn', url: 'https://linkedin.com/in/johndoe' },
    { id: 4, title: 'Instagram', url: 'https://instagram.com/johndoe' }
  ];

  return (
    // ... el resto del código JSX
  );
}
```

**¿Por qué y qué hace?**

*   **`const links = [...]`**: Estamos declarando una constante llamada `links`. Una constante es una variable que no puede ser reasignada.
*   **`[...]`**: Los corchetes definen un array. Un array es una estructura de datos que nos permite almacenar una colección de elementos (en este caso, objetos).
*   **`{ id: 1, title: '...', url: '...' }`**: Cada elemento del array es un objeto. Los objetos nos permiten agrupar datos relacionados bajo claves (como `id`, `title`, `url`).
*   **¿Por qué hacemos esto?** En lugar de tener nuestros datos de enlaces "hardcodeados" (escritos directamente) en el JSX, los estamos moviendo a una estructura de datos separada. Esto hace que nuestro código sea mucho más limpio, fácil de mantener y escalable. Si queremos añadir, eliminar o modificar un enlace, solo tenemos que cambiar este array, en lugar de buscar en el código JSX.

### 2. Renderizar la lista usando `.map()`

**Paso a paso:**

1.  En `App.jsx`, busca la sección `<section className="links-container">`.
2.  Elimina los componentes `<LinkCard>` que escribiste manualmente en el día anterior.
3.  En su lugar, vamos a usar el método `.map()` para iterar sobre nuestro array `links` y crear un componente `<LinkCard>` por cada elemento.

**Código a reemplazar:**

**Antes:**

```jsx
<section className="links-container">
  <LinkCard title="Mi Portafolio" url="https://github.com/johndoe" />
  <LinkCard title="Twitter" url="https://twitter.com/johndoe" />
  <LinkCard title="LinkedIn" url="https://linkedin.com/in/johndoe" />
</section>
```

**Después:**

```jsx
<section className="links-container">
  {links.map(link => (
    <LinkCard key={link.id} title={link.title} url={link.url} />
  ))}
</section>
```

**¿Por qué y qué hace?**

*   **`{}`**: Las llaves nos permiten escribir código JavaScript dentro de nuestro JSX.
*   **`links.map(...)`**: El método `.map()` es una función estándar de los arrays en JavaScript. Itera sobre cada elemento de un array y ejecuta una función por cada elemento, devolviendo un nuevo array con los resultados.
*   **`link => (...)`**: Esto se llama una "arrow function". Es una forma concisa de escribir una función en JavaScript. En este caso, por cada `link` (objeto) en nuestro array `links`, vamos a ejecutar el código que está dentro de los paréntesis.
*   **`<LinkCard ... />`**: Por cada `link` en el array, creamos un componente `LinkCard`.
*   **`title={link.title}` y `url={link.url}`**: En lugar de strings estáticos, ahora pasamos los datos del objeto `link` actual a las `props` del componente `LinkCard`. `link.title` accede al valor de la propiedad `title` del objeto, y `link.url` a la de `url`.
*   **`key={link.id}`**: Esta es una `prop` especial y **muy importante** en React. Cuando renderizas una lista de elementos, React necesita una forma de identificar de manera única cada elemento en esa lista para poder actualizarla eficientemente si los datos cambian. La `key` debe ser un string o un número único entre los hermanos de la lista. Por eso añadimos una propiedad `id` a nuestros objetos de enlace.

Al usar `.map()`, hemos hecho que nuestra UI sea **dinámica**. Ahora, la interfaz de usuario se genera automáticamente a partir de nuestros datos. Si añadimos un quinto enlace a nuestro array `links`, aparecerá automáticamente en la página sin necesidad de escribir más código JSX. Esta es una de las ideas más poderosas de React: **la UI es una función de tus datos**.

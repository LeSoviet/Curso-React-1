# Día 11: Manejo de Estados de Carga y Error

## Objetivos del Día

-   Mejorar la experiencia del usuario (UX) mostrando indicadores de carga y mensajes de error.
-   Añadir nuevas piezas de estado para rastrear si la aplicación está cargando o si ha ocurrido un error.
-   Usar renderizado condicional para mostrar diferentes UI según el estado actual.

## Tareas

### 1. ¿Por qué manejar estos estados?

Nuestra aplicación funciona, pero tiene un pequeño problema de experiencia de usuario. Cuando la página carga, durante un segundo (el retraso que simulamos), la sección de enlaces simplemente está vacía. Un usuario podría pensar que no hay enlaces o que algo está roto. Si ocurriera un error real al cargar los datos, el usuario no vería nada, solo una lista vacía, sin ninguna explicación.

Una buena aplicación debe comunicar claramente su estado al usuario:

-   **"Estoy trabajando en ello..."** (Estado de Carga / Loading)
-   **"Algo salió mal."** (Estado de Error)
-   **"Aquí están tus datos."** (Estado de Éxito / Success)

Vamos a implementar esto en nuestra aplicación.

### 2. Añadir Estados de Carga y Error

Necesitamos dos nuevas piezas de estado en `App.jsx` para rastrear estos nuevos estados.

**Paso a paso:**

1.  Abre `src/App.jsx`.
2.  Añade dos nuevos `useState`:
    *   `loading` (booleano): Será `true` mientras esperamos los datos, y `false` después.
    *   `error` (puede ser `null` o un objeto de error): Será `null` si todo va bien, o contendrá información del error si algo falla.

**Código a añadir en `App.jsx`:**

```jsx
function App() {
  const [links, setLinks] = useState([]);
  // 2. Nuevas piezas de estado
  const [loading, setLoading] = useState(true); // Empezamos en estado de carga
  const [error, setError] = useState(null);

  useEffect(() => {
    // ...
  }, []);

  // ...
}
```

**¿Por qué y qué hace?**

*   **`useState(true)`**: Inicializamos `loading` en `true` porque en el momento en que el componente se monta, inmediatamente comenzamos a solicitar los datos. La aplicación está, de hecho, "cargando" desde el principio.
*   **`useState(null)`**: Inicializamos `error` en `null` porque, al principio, no ha ocurrido ningún error.

### 3. Actualizar los estados en el `useEffect`

Ahora, necesitamos actualizar estos estados en los momentos apropiados dentro de nuestra lógica de carga de datos.

**Paso a paso:**

1.  En el `useEffect` de `App.jsx`, actualiza los estados `loading` y `error`.

**Código a modificar en el `useEffect` de `App.jsx`:**

```jsx
useEffect(() => {
  getLinks()
    .then(initialLinks => {
      setLinks(initialLinks);
      setError(null); // Asegurarse de que no hay errores
    })
    .catch(error => {
      console.error("Error al obtener los enlaces:", error);
      setError(error); // Guardar el error en el estado
    })
    .finally(() => {
      // finally() se ejecuta siempre, tanto si hubo éxito como si hubo error
      setLoading(false); // Dejar de cargar en cualquier caso
    });
}, []);
```

**¿Por qué y qué hace?**

*   **`setError(null)`**: En el caso de éxito (`.then`), nos aseguramos de limpiar cualquier error anterior que pudiera haber existido.
*   **`setError(error)`**: En el caso de fallo (`.catch`), guardamos el objeto de error en nuestro estado para poder mostrar un mensaje al usuario.
*   **`.finally(() => setLoading(false))`**: El método `.finally()` de las `Promises` es perfecto para esto. El código que contiene se ejecutará siempre al final, sin importar si la `Promise` se resolvió (`then`) o fue rechazada (`catch`). Es el lugar ideal para establecer `loading` a `false`, porque la operación de carga ha terminado, independientemente del resultado.

### 4. Renderizado Condicional en el JSX

Ahora que tenemos los estados, podemos usarlos para mostrar diferentes cosas en nuestra UI.

**Paso a paso:**

1.  En el `return` de `App.jsx`, modifica la parte donde renderizas `LinkList`.
2.  Añade lógica para mostrar un mensaje de carga, un mensaje de error, o la lista de enlaces, según los valores de `loading` y `error`.

**Código a modificar en el `return` de `App.jsx`:**

```jsx
return (
  <div className="container mt-5">
    <ProfileHeader {...} />
    <AddLinkForm {...} />

    {/* --- Inicio del Renderizado Condicional --- */}
    
    {loading && <p className="text-center">Cargando enlaces...</p>}

    {error && <p className="text-center text-danger">Error al cargar los enlaces.</p>}

    {!loading && !error && <LinkList links={links} onDelete={handleDeleteLink} />}

    {/* --- Fin del Renderizado Condicional --- */}
  </div>
);
```

**¿Por qué y qué hace?**

Estamos usando una característica de JavaScript llamada **evaluación de cortocircuito (short-circuit evaluation)**. Así es como funciona:

*   **`loading && <p>...</p>`**: Si `loading` es `true`, JavaScript continúa para evaluar la segunda parte de la expresión (`<p>...`) y React la renderiza. Si `loading` es `false`, la expresión se detiene y no devuelve nada.

*   **`error && <p>...</p>`**: Funciona de la misma manera. Si `error` no es `null` (es decir, es un objeto de error, que es un valor "truthy"), se renderiza el párrafo del error.

*   **`!loading && !error && <LinkList ... />`**: Esta es la condición de éxito. Solo si `loading` es falso (`!loading` es `true`) **Y** `error` es nulo (`!error` es `true`), se renderizará el componente `LinkList`.

Ahora, cuando recargues la página, verás brevemente el mensaje "Cargando enlaces...", que luego será reemplazado por la lista de enlaces. Si modificas tu `linkService.js` para que la `Promise` falle (llamando a `reject()` en lugar de `resolve()`), verás el mensaje de error en su lugar. 

Has mejorado enormemente la experiencia de usuario de tu aplicación, haciéndola más robusta y comunicativa.

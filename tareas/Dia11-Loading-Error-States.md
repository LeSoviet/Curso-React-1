# Día 11: Manejando Estados de Carga y Error

## Objetivo del Día

Las peticiones de red no son instantáneas y pueden fallar. Una buena experiencia de usuario (UX) implica mostrarle al usuario lo que está sucediendo. Hoy mejorarás tu `useEffect` para manejar un estado de "cargando" (loading) y un posible estado de "error".

## Tarea

Modificarás el componente `App` para que muestre un mensaje de "Cargando enlaces..." mientras se esperan los datos, y un mensaje de error si la carga falla.

### Paso a Paso

1.  **Añadir Estados para Carga y Error:**
    *   En `src/App.jsx`, añade dos nuevos estados: uno para `isLoading` (booleano) y otro para `error` (string o null).

    ```javascript
    // src/App.jsx
    const [isLoading, setIsLoading] = useState(true); // Empezamos cargando
    const [error, setError] = useState(null);
    ```

2.  **Actualizar `useEffect` para Manejar los Estados:**
    *   Modifica tu `useEffect` de carga de datos.
    *   Antes de llamar a la API simulada, asegúrate de que `isLoading` es `true` y `error` es `null`.
    *   En el `.then()` (cuando la promesa se resuelve con éxito), establece los datos, y luego pon `isLoading` a `false`.
    *   Encadena un `.catch()` a la promesa para manejar el caso de error. En el `catch`, pon `isLoading` a `false` y establece el mensaje de error.
    *   Para probar el error, modifica temporalmente `mockFetchLinks` para que a veces rechace la promesa.

    ```javascript
    // Función de mock modificada para pruebas
    const mockFetchLinks = () => {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          if (Math.random() > 0.2) { // 80% de éxito
            resolve(/* ...datos... */);
          } else {
            reject(new Error('No se pudieron cargar los enlaces. Inténtalo de nuevo.'));
          }
        }, 1000);
      });
    };

    // useEffect actualizado
    useEffect(() => {
      setIsLoading(true);
      setError(null);

      mockFetchLinks()
        .then(data => {
          setLinks(data);
        })
        .catch(err => {
          setError(err.message);
        })
        .finally(() => {
          setIsLoading(false);
        });
    }, []);
    ```

3.  **Renderizado Condicional en la UI:**
    *   Ahora, en la parte del `return` de `App.jsx`, usa estos nuevos estados para mostrar diferentes cosas al usuario.
    *   Justo antes de renderizar `LinkList`, añade una lógica condicional:
        *   Si `isLoading` es `true`, muestra un mensaje como "Cargando enlaces...".
        *   Si `error` tiene un valor, muestra el mensaje de error.
        *   Si no hay carga ni error, muestra la `LinkList`.

    ```jsx
    // src/App.jsx
    // ...en el return

    <div className="App">
      {/* ... ProfileHeader y AddLinkForm ... */}

      {isLoading && <p>Cargando enlaces...</p>}
      {error && <div className="alert alert-danger">{error}</div>}
      {!isLoading && !error && <LinkList links={links} onDeleteLink={handleDeleteLink} />}

    </div>
    ```

4.  **Probar los Diferentes Estados:**
    *   Recarga la página varias veces.
    *   La mayoría de las veces, deberías ver el mensaje "Cargando..." por un segundo, y luego la lista de enlaces.
    *   Eventualmente (con un 20% de probabilidad), la carga fallará. Verás el mensaje "Cargando...", y luego será reemplazado por el cuadro de alerta rojo con el mensaje de error.

Has mejorado enormemente la robustez y la experiencia de usuario de tu aplicación. Este patrón de `isLoading`/`error`/`data` es fundamental para cualquier aplicación que consuma datos externos.
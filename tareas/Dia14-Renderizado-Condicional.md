# Día 14: Renderizado Condicional

## Objetivo del Día

Mejorar la experiencia de usuario mostrando diferentes elementos de la UI basados en ciertas condiciones. Por ejemplo, mostrar un mensaje de "Cargando..." mientras se obtienen los datos, o un mensaje de "No hay enlaces" si la lista está vacía.

## Concepto Teórico

El renderizado condicional en React es simplemente usar la lógica de JavaScript (como sentencias `if`, operadores ternarios, o el operador `&&`) para decidir qué JSX renderizar.

**Patrones Comunes:**

1.  **`if/else` fuera del JSX:**
    ```jsx
    if (isLoading) {
      return <p>Cargando...</p>;
    }
    return <MiComponente />;
    ```

2.  **Operador Ternario `? :` dentro del JSX:**
    ```jsx
    <div>
      {isLoggedIn ? <p>Bienvenido</p> : <p>Por favor, inicia sesión</p>}
    </div>
    ```

3.  **Operador Lógico `&&` (Cortocircuito):** Útil cuando solo quieres renderizar algo si una condición es verdadera, y nada si es falsa.
    ```jsx
    <div>
      {unreadMessages.length > 0 &&
        <h2>Tienes {unreadMessages.length} mensajes.</h2>
      }
    </div>
    ```

## Dibujo Conceptual

Tu componente actúa como un guardia de tráfico que decide qué camino (qué UI) mostrar basado en el estado actual.

```
                          +----------------+
                          | Estado Actual  |
                          | (isLoading,    |
                          |  links.length) |
                          +-------+--------+
                                  |
              +-------------------+-------------------+
              |                                       |
      ¿isLoading es true?                       ¿links.length === 0?
              |                                       |
              v                                       v
      +---------------+                       +-------------------+
      | Renderiza     |                       | Renderiza         |
      | <Loading />   |                       | <NoLinksMessage />|
      +---------------+                       +-------------------+
```

## Tarea

Implementar renderizado condicional en `HomePage.jsx` y `AdminPage.jsx`.

### Paso a Paso

1.  **Añadir Estado de Carga en `App.jsx`:**
    *   Es una buena práctica tener un estado explícito para la carga.

    ```jsx
    // src/App.jsx
    function App() {
      const [profile, setProfile] = useState(null);
      const [links, setLinks] = useState([]);
      const [isLoading, setIsLoading] = useState(true); // <-- Nuevo estado

      useEffect(() => {
        const fetchData = async () => {
          try {
            // ... fetch ...
            setProfile(data.profile);
            setLinks(data.links);
          } catch (error) {
            console.error(error);
          } finally {
            setIsLoading(false); // <-- Se ejecuta siempre, con éxito o error
          }
        };
        fetchData();
      }, []);

      if (isLoading) {
        return <div>Cargando...</div>;
      }
      // ...
    }
    ```

2.  **Mensaje de "No hay enlaces" en `HomePage.jsx`:**
    *   Usa el operador `&&` o un ternario para mostrar un mensaje si la lista de enlaces está vacía.

    ```jsx
    // src/pages/HomePage.jsx
    // ...
    export function HomePage({ profile, links }) {
      return (
        <div>
          <ProfileHeader {...profile} />
          {links.length > 0 ? (
            <LinkList links={links} />
          ) : (
            <p>Este usuario aún no ha añadido enlaces.</p>
          )}
        </div>
      );
    }
    ```

3.  **Mejorar `AdminPage.jsx`:**
    *   Aplica la misma lógica a la lista de enlaces en la página de administración.

    ```jsx
    // src/pages/AdminPage.jsx
    // ...
    export function AdminPage({ links, onNewLink, onDeleteLink }) {
      return (
        <div>
          {/* ... */}
          <h3>Mis Enlaces Actuales:</h3>
          {links.length > 0 ? (
            <ul>
              {links.map(link => (
                // ...
              ))}
            </ul>
          ) : (
            <p>No tienes enlaces. ¡Añade el primero!</p>
          )}
        </div>
      );
    }
    ```

### Verifica el Resultado

-   Al recargar, verás el mensaje "Cargando...".
-   Ve a la página de admin y elimina todos los enlaces. Verás el mensaje "No tienes enlaces...".
-   Añade un enlace y la lista aparecerá.
-   Ve a la página de inicio. Si no hay enlaces, verás el mensaje correspondiente.

Tu aplicación ahora es más robusta y comunica su estado al usuario de forma mucho más clara.

# Día 14: Técnicas Avanzadas de Renderizado Condicional

## Objetivos del Día

-   Aprender a usar el operador ternario (`? :`) para lógica condicional de tipo `if/else` en JSX.
-   Manejar el caso en que la lista de enlaces está vacía después de cargar.
-   Mejorar la experiencia de usuario mostrando mensajes informativos en lugar de una pantalla en blanco.

## Tareas

### 1. Más Allá del Operador `&&`

El operador `&&` es genial para casos simples de "si esto es verdad, renderiza aquello".

```jsx
loading && <p>Cargando...</p>
```

Pero, ¿qué pasa si queremos una lógica de `if/else`? Por ejemplo: "**Si** está cargando, muestra el spinner; **si no**, muestra los datos". Para esto, el operador `&&` no es suficiente. Podríamos usar dos líneas separadas, pero hay una forma más limpia: el **operador ternario**.

**Sintaxis del Operador Ternario:**

```javascript
condicion ? expresionSiVerdadero : expresionSiFalso
```

Es una forma compacta de escribir una declaración `if/else` en una sola línea, y funciona perfectamente dentro de JSX.

### 2. Manejando el Estado de "Lista Vacía"

Actualmente, si un usuario elimina todos sus enlaces, la sección de enlaces simplemente desaparecerá, dejando un espacio en blanco. Esto no es ideal. Sería mejor mostrar un mensaje como: "Aún no tienes enlaces. ¡Añade uno!".

Este es un tercer estado que no habíamos considerado:
1.  Cargando
2.  Error
3.  Éxito con datos
4.  **Éxito sin datos (lista vacía)**

Vamos a implementar la lógica para mostrar este mensaje.

**Paso a paso:**

1.  Abre el componente `src/components/LinkManager.jsx`, que ahora controla la renderización de la lista.
2.  Modifica la sección de renderizado condicional para manejar este nuevo caso.

**Código a modificar en `LinkManager.jsx`:**

```jsx
// ... dentro del return de LinkManager ...

{/* Usaremos una función para mantener el JSX limpio */}
const renderContent = () => {
  // 1. Primero, manejar el estado de carga
  if (loading) {
    return <p className="text-center">Cargando enlaces...</p>;
  }

  // 2. Luego, manejar el estado de error
  if (error) {
    return <p className="text-center text-danger">Error al cargar los enlaces.</p>;
  }

  // 3. Si no hay carga ni error, comprobar si la lista está vacía
  if (links.length === 0) {
    return (
      <div className="text-center p-4 border rounded">
        <p className="mb-0">Aún no tienes enlaces. ¡Añade uno usando el formulario de arriba!</p>
      </div>
    );
  }

  // 4. Si todo lo demás falla, significa que tenemos datos para mostrar
  return <LinkList links={links} onDelete={handleDeleteLink} />;
};

return (
  <div>
    <AddLinkForm onAddLink={handleAddLink} />
    <div className="mt-4">
      {renderContent()} { /* Llamamos a la función para renderizar el contenido */}
    </div>
  </div>
);
```

**¿Por qué y qué hace?**

*   **`const renderContent = () => { ... }`**: Hemos movido nuestra lógica condicional a una función dentro del componente. Esto es un patrón muy útil para evitar que el bloque `return` principal se vuelva un desorden de lógica ternaria anidada y operadores `&&`. Hace que el código sea mucho más fácil de leer y seguir.

*   **`if (loading) { ... }`**: La estructura `if/else if/else` es mucho más clara para manejar múltiples condiciones excluyentes.

*   **`if (links.length === 0)`**: Después de que la carga ha terminado y sabemos que no hay errores, comprobamos si el array `links` está vacío. Si lo está, devolvemos un componente JSX con un mensaje amigable para el usuario.

*   **`return <LinkList ... />`**: Solo si todas las condiciones anteriores son falsas, llegamos al caso de éxito donde renderizamos la lista de enlaces.

*   **`{renderContent()}`**: En nuestro JSX principal, simplemente llamamos a esta función. La función se ejecuta, determina qué JSX debe ser devuelto, y React lo renderiza en ese lugar.

### 3. Usando el Operador Ternario (Alternativa)

También podrías haber logrado un resultado similar usando operadores ternarios anidados, aunque puede volverse más difícil de leer.

**Ejemplo con ternarios:**

```jsx
// Dentro del return principal
<div className="mt-4">
  {loading ? (
    <p className="text-center">Cargando...</p>
  ) : error ? (
    <p className="text-center text-danger">Error.</p>
  ) : links.length === 0 ? (
    <p className="text-center">No hay enlaces.</p>
  ) : (
    <LinkList links={links} onDelete={handleDeleteLink} />
  )}
</div>
```

Ambos enfoques son válidos. La función `renderContent` suele ser preferida cuando la lógica se vuelve compleja (más de una o dos condiciones) porque mejora la legibilidad, mientras que el ternario es excelente para cambios rápidos de `if/else`.

Al implementar esta lógica, has hecho tu aplicación más inteligente y amigable. Ahora proporciona una retroalimentación clara al usuario en todos los escenarios posibles: carga, error, éxito con datos y éxito sin datos.
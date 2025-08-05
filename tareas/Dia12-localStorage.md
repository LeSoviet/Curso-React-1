# Día 12: Persistiendo el Estado con `localStorage`

## Objetivos del Día

-   Aprender qué es `localStorage` y cómo funciona.
-   Usar `useEffect` para **guardar** la lista de enlaces en `localStorage` cada vez que cambie.
-   Modificar la lógica de carga inicial para que lea desde `localStorage` como primera opción, antes de usar los datos por defecto.

## Tareas

### 1. ¿Qué es `localStorage`?

`localStorage` es una API del navegador que permite a las aplicaciones web guardar datos de tipo clave-valor (`key: value`) de forma persistente en el navegador del usuario. 

**Características clave:**

-   **Persistente:** Los datos guardados en `localStorage` **no se borran** cuando el usuario cierra la pestaña o el navegador. Permanecen ahí hasta que son eliminados explícitamente por la aplicación o por el usuario.
-   **Específico del Origen:** Los datos están ligados al "origen" (el protocolo, el host y el puerto). Esto significa que los datos de `www.misitio.com` no pueden ser accedidos por `www.otrositio.com`.
-   **Solo Strings:** `localStorage` solo puede almacenar strings. Si quieres guardar un objeto o un array, primero debes convertirlo a un string (usando `JSON.stringify()`) y luego, al leerlo, convertirlo de nuevo a un objeto (usando `JSON.parse()`).

Es ideal para guardar preferencias de usuario, el contenido de un carrito de compras, o, como en nuestro caso, los datos de nuestra aplicación para que no se pierdan al recargar la página.

### 2. Guardando los enlaces en `localStorage`

Queremos guardar nuestra lista de enlaces cada vez que se modifica (al añadir o eliminar un enlace). El lugar perfecto para esta lógica es un `useEffect` que dependa del estado `links`.

**Paso a paso:**

1.  Abre `src/App.jsx`.
2.  Localiza el `useEffect` que ya tienes que se ejecuta cuando `links` cambia (el que muestra el `console.log` y actualiza el `document.title`).
3.  Añade la lógica para guardar en `localStorage` dentro de ese mismo `useEffect`.

**Código a modificar en `App.jsx`:**

```jsx
// ...
useEffect(() => {
  // Este efecto se ejecuta cuando 'links' cambia.
  console.log('La lista de enlaces ha cambiado:', links);
  document.title = `LinkHub (${links.length} enlaces)`;

  // 3. Guardar en localStorage
  // Solo intentamos guardar si la carga inicial ya terminó, para no guardar un array vacío al principio.
  if (!loading) {
    localStorage.setItem('linkhub_links', JSON.stringify(links));
    console.log('Enlaces guardados en localStorage.');
  }
}, [links, loading]); // Añadimos 'loading' a las dependencias
// ...
```

**¿Por qué y qué hace?**

*   **`localStorage.setItem(key, value)`**: Este es el método para guardar datos. 
    *   `'linkhub_links'`: Es la `key` (clave) que usaremos para identificar nuestros datos. Es bueno usar un prefijo para evitar colisiones con otras aplicaciones.
    *   `JSON.stringify(links)`: Es el `value` (valor). Convertimos nuestro array de objetos `links` a un formato de string JSON para poder guardarlo.
*   **`if (!loading)`**: Añadimos esta condición para evitar un problema: cuando la app carga, `links` es `[]` y `loading` es `true`. Luego, cuando los datos llegan, `links` se actualiza y `loading` se vuelve `false`. Queremos guardar solo después de que este proceso haya terminado. Al incluir `loading` en las dependencias, este `useEffect` se ejecutará también cuando `loading` cambie, asegurando que guardemos el estado correcto.
*   **`[links, loading]`**: Hemos añadido `loading` al array de dependencias porque ahora lo estamos usando dentro del `useEffect`.

### 3. Cargando los enlaces desde `localStorage`

Ahora que estamos guardando los datos, necesitamos leerlos cuando la aplicación se carga. La idea es: "Intenta cargar desde `localStorage`. Si no hay nada, entonces usa los datos por defecto de la API simulada".

**Paso a paso:**

1.  Modifica el `useEffect` que se ejecuta una sola vez (el que tiene `[]` como dependencias).
2.  Dentro de él, antes de llamar a la API, comprueba si existen datos en `localStorage`.

**Código a modificar en el `useEffect` de carga inicial en `App.jsx`:**

```jsx
useEffect(() => {
  // Este efecto se ejecuta solo una vez, al montar el componente.
  console.log("Componente montado. Buscando enlaces...");

  // 2. Intentar cargar desde localStorage primero
  const savedLinks = localStorage.getItem('linkhub_links');

  if (savedLinks) {
    console.log("Enlaces encontrados en localStorage. Cargando...");
    setLinks(JSON.parse(savedLinks));
    setLoading(false); // Ya tenemos los datos, no hay que cargar más
  } else {
    // Si no hay nada en localStorage, entonces llamar a la API
    console.log("No hay enlaces en localStorage. Obteniendo de la API...");
    getLinks()
      .then(initialLinks => {
        setLinks(initialLinks);
        setError(null);
      })
      .catch(error => {
        console.error("Error al obtener los enlaces:", error);
        setError(error);
      })
      .finally(() => {
        setLoading(false);
      });
  }
}, []); // El array vacío sigue siendo correcto aquí
```

**¿Por qué y qué hace?**

*   **`localStorage.getItem('linkhub_links')`**: Este es el método para leer datos. Devuelve el valor guardado como un string, o `null` si la clave no existe.
*   **`if (savedLinks)`**: Si `savedLinks` no es `null`, significa que encontramos datos.
*   **`setLinks(JSON.parse(savedLinks))`**: Convertimos el string JSON de vuelta a un array de JavaScript y lo usamos para establecer nuestro estado.
*   **`setLoading(false)`**: Como hemos cargado los datos de forma síncrona desde `localStorage`, podemos establecer `loading` a `false` inmediatamente.
*   **`else { ... }`**: Si no se encontraron datos en `localStorage`, entonces y solo entonces, ejecutamos nuestra lógica original de llamada a la API.

Ahora, tu aplicación tiene memoria. Añade o elimina algunos enlaces y luego recarga la página. Verás que tus cambios persisten. Has hecho que la aplicación sea mucho más robusta y útil para el usuario final.

# Día 12: Persistiendo Estado con `localStorage`

## Objetivo del Día

Hoy harás que tu aplicación se sienta mucho más permanente. Aprenderás a usar la `localStorage` del navegador para guardar los enlaces del usuario. De esta manera, si el usuario cierra o recarga la pestaña, sus enlaces seguirán ahí cuando vuelva.

## Tarea

Utilizarás `useEffect` para dos cosas: guardar los enlaces en `localStorage` cada vez que cambien, y leerlos de `localStorage` cuando la aplicación se carga por primera vez.

### Paso a Paso

1.  **Leer el Estado Inicial desde `localStorage`:**
    *   En `src/App.jsx`, vamos a modificar la inicialización del estado `links`.
    *   En lugar de `useState([])`, le pasaremos una función. React ejecutará esta función **solo una vez** para obtener el estado inicial.
    *   Dentro de esta función, intenta obtener los enlaces de `localStorage`. Si existen, pársalos de vuelta a formato de objeto con `JSON.parse()`. Si no existen, devuelve un array vacío.

    ```jsx
    // src/App.jsx

    function App() {
      const [links, setLinks] = useState(() => {
        const savedLinks = localStorage.getItem('my-links');
        return savedLinks ? JSON.parse(savedLinks) : [];
      });

      // ...otros estados
    }
    ```
    *   **Importante:** Ya no necesitas el `useEffect` que cargaba los datos de `mockFetchLinks`. Puedes eliminarlo, junto con los estados `isLoading` y `error` por ahora para simplificar.

2.  **Guardar el Estado en `localStorage` con `useEffect`:**
    *   Necesitamos un efecto que se ejecute *cada vez que la lista de enlaces cambie*.
    *   Crea un nuevo `useEffect`.
    *   La función del efecto guardará la lista actual de `links` en `localStorage`. Como `localStorage` solo puede guardar strings, necesitas convertir tu array de objetos a un string con `JSON.stringify()`.
    *   El array de dependencias de este efecto debe contener `[links]`. Esto le dice a React: "ejecuta este efecto cada vez que el valor de `links` cambie".

    ```jsx
    // src/App.jsx
    // ...después de la inicialización de estados

    useEffect(() => {
      localStorage.setItem('my-links', JSON.stringify(links));
    }, [links]); // Dependencia: el estado de los enlaces

    // ...resto del componente
    ```

3.  **Limpieza y Prueba:**
    *   Asegúrate de haber eliminado el `useEffect` anterior que usaba `mockFetchLinks`, así como los estados `isLoading` y `error` y su renderizado condicional. Queremos centrarnos solo en la persistencia local.
    *   Abre tu aplicación.
    *   Añade y elimina algunos enlaces.
    *   **Recarga la página.** Tus enlaces deberían seguir ahí.
    *   Cierra la pestaña del navegador y vuelve a abrirla. Los enlaces persistirán.

4.  **(Opcional) Ver en las Herramientas de Desarrollador:**
    *   Abre las herramientas de desarrollador de tu navegador (F12 o clic derecho > Inspeccionar).
    *   Ve a la pestaña "Application" (o "Almacenamiento" en algunos navegadores).
    *   En el menú de la izquierda, busca "Local Storage" y selecciona tu sitio (`http://localhost:5173`).
    *   Verás una clave `my-links` y su valor, que es la versión en string de tu array de enlaces.

¡Genial! Tu aplicación ahora tiene memoria. Has aprendido a sincronizar el estado de React con una API de almacenamiento del navegador, una técnica muy útil para mejorar la experiencia del usuario.
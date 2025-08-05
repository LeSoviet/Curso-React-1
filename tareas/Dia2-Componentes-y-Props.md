# Día 2: Componentes y Props

## Objetivo del Día

Hoy vamos a dar un paso fundamental en React: aprender a crear y utilizar componentes reutilizables. Refactorizaremos nuestro código para que sea más modular y fácil de mantener.

## Tarea

Tu tarea es crear dos nuevos componentes: `ProfileHeader` y `LinkList`.

### Paso a Paso

1.  **Crear la Carpeta de Componentes:**
    *   Dentro de la carpeta `src`, crea una nueva carpeta llamada `components`.

2.  **Crear el Componente `ProfileHeader`:**
    *   Dentro de `src/components`, crea un nuevo archivo llamado `ProfileHeader.jsx`.
    *   Este componente se encargará de mostrar la imagen de perfil, el nombre de usuario y la biografía.
    *   Mueve el código JSX correspondiente desde `App.jsx` a `ProfileHeader.jsx`.
    *   El componente `ProfileHeader` debe aceptar `props` para la imagen, el nombre y la biografía, para que podamos reutilizarlo con diferentes datos.

    ```jsx
    // src/components/ProfileHeader.jsx

    function ProfileHeader(props) {
      return (
        <header>
          <img src={props.imageUrl} alt="Foto de perfil" />
          <h1>{props.username}</h1>
          <p>{props.bio}</p>
        </header>
      );
    }

    export default ProfileHeader;
    ```

3.  **Crear el Componente `LinkList`:**
    *   Dentro de `src/components`, crea un archivo `LinkList.jsx`.
    *   Este componente mostrará la lista de enlaces. Por ahora, puedes poner algunos enlaces de ejemplo directamente en el JSX.
    *   Más adelante, haremos que reciba la lista de enlaces como una `prop`.

    ```jsx
    // src/components/LinkList.jsx

    function LinkList() {
      return (
        <section>
          <ul>
            <li><a href="#">Mi Portafolio</a></li>
            <li><a href="#">Mi LinkedIn</a></li>
            <li><a href="#">Mi Twitter</a></li>
          </ul>
        </section>
      );
    }

    export default LinkList;
    ```

4.  **Actualizar `App.jsx`:**
    *   Importa los nuevos componentes `ProfileHeader` y `LinkList` en `App.jsx`.
    *   Utiliza los componentes en el `return` de `App`, pasándole las `props` necesarias a `ProfileHeader`.

    ```jsx
    // src/App.jsx
    import './App.css';
    import ProfileHeader from './components/ProfileHeader';
    import LinkList from './components/LinkList';

    function App() {
      const profileData = {
        imageUrl: 'https://via.placeholder.com/150',
        username: '@tunombredeusuario',
        bio: 'Desarrollador Frontend en formación.'
      };

      return (
        <div className="App">
          <ProfileHeader 
            imageUrl={profileData.imageUrl}
            username={profileData.username}
            bio={profileData.bio}
          />
          <LinkList />
        </div>
      );
    }

    export default App;
    ```

5.  **Ver el Resultado:**
    *   Si tienes el servidor de desarrollo corriendo, los cambios se deberían reflejar automáticamente. Si no, ejecuta `npm run dev`.
    *   La apariencia no debería haber cambiado, pero ahora nuestro código está mucho mejor organizado.

¡Buen trabajo! Hoy has aprendido uno de los conceptos más importantes de React.

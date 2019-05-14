---
title: Cómo proteger tus rutas con React.Context
---

En un [artículo anterior](https://gonzalo-chumillas.github.io/jwt-json-web-token/) analizamos una aplicación REST que otorgaba "tokens de acceso". Cuando el cliente se identificaba con éxito en el sistema, éste le proporcionaba un token que servía como credencial para futuras peticiones.

Y en esta ocasión vamos a analizar la contraparte. Es decir, una aplicación desarrollada en [React](https://reactjs.org/) que se identifica en el sistema anterior y obtiene un token de acceso. Y en particular usaremos `React.Context` para almacenar dicho token y proporcionarlo a los diferentes componentes de la aplicación.

Para situarnos, [React.Context](https://reactjs.org/docs/context.html) es una técnica de programación que nos permite transmitir información desde los componentes de mayor nivel (componentes padres) hasta los de menor nivel (componentes hijos) sin necesidad de pasarlos como parámetros en los constructores.

Podéis descargar la aplicación en formato ZIP [aquí](https://github.com/gchumillas/cmsystem-client/archive/0.1.0.zip).

## Estructura de la aplicación

Pero antes de continuar es conveniente echar un vistazo a la estructura de la aplicación.

```text
📁 components   ⇒ Componentes comunes
  📁 dialogs
  📁 http
  📁 messages
    Body.js
📁 core         ⇒ Clases y funciones internas
  context.js
  http.js
  storage.js
📁 providers    ⇒ Funciones proveedoras
  auth.js
  home.js
📁 translations ⇒ Traducciones
  en.json
  es.json
📁 views        ⇒ Vistas o 'layouts'
  📁 common     ⇒ Componentes comunes a todas las vistas
    Header.js
  Home.js
  Login.js
Main.js        ⇒ Aplicación principal
```

De los archivos anteriores, los que analizaremos a continuación serán `core/context.js`, `Main.js` y `views/Login.js`.

## Proporcionando el contexto (Context.Provider)

Lo primero que debemos hacer es crear un contexto desde el que transmitir valores o funciones al resto de los componentes de menor nivel o "componentes hijos".

El archivo `core/context.js` contiene la constante `Context`, que representa el contexto de la aplicación.

```js
// core/context.js
import React from 'react'

export const Context = React.createContext()
```

Que a su vez será usada por la clase `Main` para proporcionar un contexto al resto de los componentes hijos:
```js
// Main.js
render => () {
  return (
    <Context.Provider value={% raw %}{{{% endraw %}
      token,
      signIn: this.signIn,
      signOut: this.signOut,
      http,
      isPending,
      setHttpError: this.setHttpError
    {% raw %}}}{% endraw %}>
      {/* ... more components ... */}
    </Context.Provider>
  )
}
```

Y en particular inyectaremos los siguientes valores y funciones:

1. `token`: Una cadena de texto que representa el token de acceso.
2. `signIn(username, password, rememberMe)`: Una función asíncrona para autenticarnos y obtener un token de acceso.
3. `signOut()`: Para salir del sistema.
4. `http`: Una interfaz para lanzar peticiones asíncronas (contiene los métodos `get`, `post`, `put`, `patch` y `delete`)
5. `isPending`: Para determinar el estado de la petición en curso.
6. `setHttpError(code, message)`: Para sobrescribir los posibles mensajes de error HTTP.

## Consumiendo el contexto (Context.Consumer)

Y una vez creado el contexto es hora de consumirlo. Y ello se consigue mediante el componente [Context.Consumer](https://reactjs.org/docs/context.html#contextconsumer).

El problema de hacerlo de la forma anterior es que el contexto solo estaría disponible en la función `render()`. Y a nosotros nos interesa que esté disponible durante todo el ciclo de vida de nuestro componente.

Es por ello que hemos creado la función `withContext()` que encontraréis en el archivo `core/context.js`:

```js
// core/context.js
import React from 'react'

export const Context = React.createContext()

export function withContext(Component) {
  return class extends React.Component<Props> {
    render = () => {
      return (
        <Context.Consumer>
          {state => {
            return <Component {...state} {...this.props} />
          }}
        </Context.Consumer>
      )
    }
  }
}
```

La función anterior consiste en un componente [HOC](https://reactjs.org/docs/higher-order-components.html) y nos permite 'inyectar' determinadas propiedades. En nuestro caso, inyectaremos las propiedades del contexto definidas en la clase `Main`:

```js
// views/Login.js
export default withStyles(styles)(withContext(Login))
```

Posteriormente podemos hacer uso de esas propiedades. Por ejemplo, la función `signIn()` es una propiedad que definimos en la clase `Main` y que pasamos a través del contexto:

```js
// views/Login.js
handleSubmit = () => {
  const { signIn } = this.props
  const { username, password, remember } = this.state

  signIn(username, password, remember)
}
```

## Proteger las rutas

Una ruta no es más que otro componente, que a su vez puede consumir las propiedades del contexto:

```js
// Main.js
const ProtectedRoute = withContext(class extends React.Component<{
  token: string,
  component: React$ElementType
}> {
  render = () => {
    const { token, component: Component, ...rest } = this.props

    return <Route {...rest}
      render={props => {
        return token ? <Component {...props} /> : <Login />
      }} />
  }
})
```

Proteger una ruta es tan sencillo como examinar la propiedad `token` y mostrar la vista `Login` en caso de que no esté definida.

## Conclusión

Como podéis apreciar la idea subyacente es muy sencilla. Simplemente guardamos el token en un contexto que posteriormente será usado por el resto de los componentes de menor nivel. Quizá la parte más compleja sea el uso de la técnica [HOC](https://reactjs.org/docs/higher-order-components.html) para inyectar las propiedades del contexto a los "componentes hijos".

Por cierto, no hemos necesitado usar Redux ni su tan controvertida [única fuente del conocimiento](https://redux.js.org/introduction/three-principles#single-source-of-truth).

Toma ya, Chuck Norris!

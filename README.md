

# Context API

Habilitar el acceso de variables a todos los componentes que esten envueltos en el Proveedor de Contexto.

Context:
Provider:

`createContext`

`useReducer()`

```Javascript
type ActivityProviderProps = {
  children: ReactNode
}

type ActivityContextProps = {
  state: ActivityState
  dispatch: Dispatch<ActivityActions>
}

export const ActivityContext = createContext<ActivityContextProps>({} as )

export const ActivityProvider = ({children} : ActivityProviderProps) => {

  const [state, dispatch] = useReducer(activityReducer, initialState)

  return (
    <ActivityContext.Provider
      value={{
        state,
        dispatch,
      }}
    >
      {children}
    </ActivityContext.Provider>
  )

}
```
Para envolver la aplicación en el Contexto, se tiene que utilizar el ContextProvider.

```Javascript
ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <ActivityProvider>
      <App /> 
    </ActivityProvider>
  </React.StrictMode>,
)
``` 

# Reducer & CustomHook

Un reducer puede ser creado con el hook ``useReducer`` de React, con ello podemos empaquetar nuestra logica para componentes y estados de la aplicación.

Con un CustomHook, como ``useActivity`` en este caso, se crea una interfaz de facil acceso a las acciones del reducer.

Con este podemos, montar nuestro Contexto, tambien controlar de que este siendo llamado correctamente en la aplicación.

```JavaScript
import { useContext } from "react";
import { ActivityContext } from "../context/ActivityContext";

export const seActivity = () => {
  const context = useContext(ActivityContext)

  if(!context) {
    throw new Error('Error, the useActivity hook must be used inside an ActivityProvider')
  }

  return context
}
```

# Migración de solo Reducer con Prop Drilling a Context

Se remueven las instancias de necesitar props en los componentes y simplemente se llama al customHook `useActivity` dentro del mismo componente para acceder a la información.
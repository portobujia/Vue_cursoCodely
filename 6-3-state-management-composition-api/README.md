# Vue 3: State Management with Composition API

## Project setup

```
npm install
```

### Compiles and hot-reloads for development

```
npm run serve
```

### Compiles and minifies for production

```
npm run build
```

### Lints and fixes files

```
npm run lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

### Descripción

Otra alternativa a la hora de trabajar con estado global en nuestras aplicaciones Vue 3 es aprovechar las facilidades que nos da la Composition API. Siguiendo con el mismo ejemplo de la versión anterior, podemos crear nuestro User.ts exportando una función que de al componente acceso al estado global, que dejaremos fuera de la función, así como los métodos login y logout:


const user: Ref<User | null> = ref(null);

export function useUser() {
  function login() {
    // ...
  }

  function logout() {
    // ...
  }

  return {
    user: readonly(user),
    isLogged: computed(() => !!user.value),
    login,
    logout,
  };
}

Como en el ejemplo anterior, tenemos que acordarnos de exportar user como readonly.


Desde el componente que necesite acceder a nuestro estado, solo tenemos que llamar esta función dentro del método setup:


export default defineComponent({
  async setup() {
    const { user } = useUser();

    return {
      userImage: user.image,
    };
  },
});
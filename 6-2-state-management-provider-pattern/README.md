# Vue 3: State Management with Provider Pattern

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


Como hemos visto anteriormente, usando provide desde un componente podemos proveer a todos sus descendientes de acceso a su estado. Esto nos abre las puertas a usar provide/inject como una herramienta de control de estado global de nuestra aplicación.


Por ejemplo, vamos a definir un componente cuya única responsabilidad va a ser proveer del estado del usuario:


export default defineComponent({
  setup() {
    const user: Ref<User | null> = ref(null);
    const isLogged = computed(() => !!user.value);

    provide("USER", user);
    provide("USER_IS_LOGGED", isLogged);
  },
  render() {
    return this.$slots.default?.();
  },
});

Cuando trabajamos con estado global, es muy importante no perder el control de ese estado. Con el ejemplo anterior, cualquier componente podría alterar la variable user, provocando bugs en nuestro código si se altera de forma no esperada. Para evitar esto, desde nuestro provider tenemos que exportar la variable como readonly, otra función de la API de Reactivity que evitará que se pueda alterar desde cualquier componente. Además, vamos a proveer a los componentes de las funciones login y logout, para dejar que los componentes puedan modificar el estado solo de la forma adecuada:


export default defineComponent({
  setup() {
		const user: Ref<User | null> = ref(null);
    const isLogged = computed(() => !!user.value);

    provide("USER", user);
    provide("USER_IS_LOGGED", isLogged);

    function login() {
      // ...
    }

    function logout() {
      // ...
    }

    provide("USER_LOG_IN", login);
    provide("USER_LOG_OUT", logout);
  },
  render() {
    return this.$slots.default?.();
  },
});

Para que otros componentes puedan acceder al estado que hemos definido, vamos a añadir este componente como parent de los componentes que lo van a necesitar:


<ProvideUser>
  <header class="header">
    <User />
  </header>
  <CourseDetail />
</ProvideUser>
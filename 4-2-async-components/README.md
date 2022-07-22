# Vue 3: Async Components with Suspense

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

Ya en Vue 2 podíamos cargar componentes de forma asíncrona. De esta forma conseguimos reducir el tamaño del bundle principal y mejorar la performance de nuestra aplicación. En Vue 3, la única diferencia a la hora de cargar componentes asíncronamente es que tendremos que usar la función defineAsyncComponent:


import { defineAsyncComponent, defineComponent } from "vue";

export default defineComponent({
  components: {
    Chat: defineAsyncComponent(() => import("./Chat.vue")),
  },
})

¿Pero qué pasa cuando un componente necesita realizar tareas asíncronas antes de renderizarse? Por ejemplo, nuestro componente Chat tiene que conectarse al servidor y esto puede llegar a tardar bastante tiempo. En Vue 2, solíamos trabajar esta lógica dentro del mismo componente, seteando una propiedad loading y cambiar en el template si mostramos un spinner o el html que forma el chat. En Vue 3, gracias a Suspense, podemos delegar esta tarea a Vue y evitar replicar esta lógica en todos los componentes que necesiten realizar tareas asíncronas.


<Suspense v-if="isLogged">
  <template #default>
    <Chat />
  </template>
  <template #fallback>
    <Loader />
  </template>
</Suspense>

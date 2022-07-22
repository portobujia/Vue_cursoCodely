# Vue 3: Teleport

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

Cuando trabajamos con componentes, queremos tener la lógica, mark-up y estilos agrupados según responsabilidades. Pero a veces hay partes del template que, aunque por lógica deberían estar en el mismo componente, técnicamente es necesario que se renderizen en otra parte del DOM.


El ejemplo más común es el uso de modales. Tenemos un componente Course que al hacer click al thumbnail, abrirá un modal con la información del curso:


<article
  role="button"
  :aria-label="`Ver información sobre ${course.title}`"
  @click="isOpen = true"
>
  <img :src="`/cursos/${course.img}`" :alt="course.title" />
</article>
<Modal v-if="isOpen">
  <CourseInfo :course="course" />
</Modal>

El modal se posiciona con position: fixed, que significa que en la mayoría de casos, se posicionará respecto al viewport. El problema viene por ejemplo si usamos este componente Course dentro de un carousel. El carousel cambia su posición con transform, creando un contexto de posicionamiento distinto y provocando que el modal no se posicione como esperamos. Para evitar eso, podríamos extraer el modal fuera del componente y emitir un evento para abrirlo desde otro componente. Pero en Vue 3, podemos renderizar partes de nuestro componente en otros puntos del DOM sin necesidad de fragmentar nuestro componente con Teleport. Encapsulando nuestro modal en un Teleport, podemos teletransportarlo a otro elemento con la propiedad to:


<teleport to="body">
  <section class="modal" @click.stop>
    <slot />
  </section>
</teleport>
# Vue 3: `v-model` between components

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

En Vue 2, usamos v-model sobretodo para trabajar con inputs, pero también se puede usar para comunicarnos entre componentes. Lo que hace v-model es crearnos un shortcut para bindear la propiedad value y actualizarla al recibir un evento, es decir <Search v-model="searchQuery" /> es lo mismo que hacer <Search :value="searchQuery" @input="searchQuery = $event" />.


En Vue 2 también podíamos hacer lo mismo usando el modifyer .sync al hacer binding de una prop:


<Search :query.sync="searchQuery" />

Esto sería equivalente a hacer:


<Search :query="searchQuery" @update:query="searchQuery = $event" />

La ventaja sobre v-model es que podíamos bindear y actualizar múltiples props, pero lo que resultaba confuso es tener varias formas de hacer este binding y actualización de propiedades entre componentes.


En Vue 3, se ha unificado este comportamiento, deprecando el modifier .sync y actualizando v-model para poder asignarle un nombre a la propiedad:


<Search v-model:query="searchQuery" />
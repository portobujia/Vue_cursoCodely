# Vue 3: Changes in `<style>`

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

En ocasiones necesitamos añadir estilos a nuestros componentes que dependen de algun valor que está en el estado del componente. En el vídeo usamos como ejemplo que cada curso tiene un color y una imagen de fondo asignados que nos vienen de una API. También es habitual cuando tenemos que animar algo con CSS que depende de la posición del scroll o del tamaño dinámico de los elementos, cosas que solo podemos determinar con JS.


Las variables CSS o custom properties nos ayudan a estilar este tipo de cosas más fácilmente, pudiendo definir las variables desde JS y usarlas en CSS sin tener que añadir propiedades en atributos style, que perjudican la mantenibilidad de nuestros estilos. Vue 3 nos permite pasar variables del estado del componente a los estilos de forma más clara y limpia.
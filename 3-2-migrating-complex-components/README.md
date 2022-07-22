# Vue 3: Migrating complex components to Composition API for better separation of concerns

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

Otro de los grandes beneficios de la Composition API es que hace más fácil el trabajar con componentes complejos.


En este vídeo vamos a ver un componente en Options API que tiene 3 grandes responsabilidades


Cursos: este componente va a hacer una llamada a la API en mounted para obtener la información sobre cursos y guardarla en el estado local como courses.
Filtrar: los usuarios pueden filtrar los cursos por categoría.
Buscar: Además de filtrar, también queremos que los usuarios puedan buscar por título.

Estas tres responsabilidades se encuentran mezcladas en todas las opciones de la Options API: estan en el estado, en computed, en methods… No seguimos una buena separación por responsabilidades y cuanto más crezca el componente, más difícil será de entender y mantener.


El primer paso será convertir este componente a Composition API, como ya hemos hecho en vídeos anteriores. Vemos como la mejora es relativa: aún hay mucho código que podremos extraer, para conseguir simplificar nuestro componente al máximo. ¡Lo veremos en el siguiente vídeo!
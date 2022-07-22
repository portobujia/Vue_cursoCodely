# Vue 3: Avoid prop-drilling with Provide/Inject

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

Cuando queremos pasar datos de un componente a otro que es descendiente, trabajamos con props. Esto provoca que a veces, si el componente que necesita cierta prop está muy anidado, tengamos que pasar esa propiedad a través de componentes que no la necesitan. Eso resulta tedioso y es fácil perder esa propiedad en la cadena. Por ejemplo, tenemos esta estructura:


Courses
└─ Filters
   └─ FilterControls
      ├─ TagFilter
      ├─ Search
      └─ TotalCourses

El componente TotalCourses necesita la prop CoursesLength, la cantidad de cursos que está mostrando el componente Courses, pero para obtenerla tenemos que pasarla por los componentes Filters y FiltersControls, aunque estos no la usen.


Para evitar esto, con Vue 3 podemos pasar propiedades usando provide y inject


Desde el componente Courses, hacemos provide de la propiedad computada coursesLength:


import { defineComponent, provide } from "vue";
import { useCourses } from "./useCourses";

export default defineComponent({
  setup() {	
		const { courses } = useCourses();
		const coursesLength = computed(() => courses.value.length);
    provide("coursesLength", coursesLength);
		
		return { courses };
  }
})

Ahora, desde cualquier descendente podemos inyectar esa propiedad, sin importar lo anidado que esté:


// TotalCourses.vue

import { defineComponent, inject } from "vue";

export default defineComponent({
  setup() {
    return {
      coursesLength: inject("coursesLength"),
    };
  },
});

Hay que tener en cuenta que, si bien con este método conseguimos evitar el prop-drilling, pueden surgir otros problemas, ya que estamos siendo menos explícitos y perdemos tipado, así que hay que valorar cuando vale la pena hacer este trade-off.

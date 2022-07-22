# Vue 3: Options API Overview

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

La Options API es la forma en la que estamos acostumbrados a trabajar en Vue 2. Definimos el estado del componente con la función data, definimos propiedades computadas derivadas de este estado en computed, funciones en methods, y los comportamientos que deben pasar en cada momento del ciclo de vida del componente los definimos en los hooks como mounted o created.


Esto, en un componente pequeño, funciona bien y aporta una estructura clara, pero tiene ciertos inconvenientes que a medida que nuestra aplicación crezca van a ser más evidentes:


Exponemos todo al template: el template puede usar cualquier método o propiedad del estado, y no podemos comunicar a otros devs de forma clara si ciertas propiedadas estan pensadas solo para usarse en la parte de lógica del componente
Control de la reactividad: todo lo que definimos en data va a ser reactivo, es decir, se actualizará automáticamente en el template y en cualquier propiedad computada que dependa de ello. Esto normalmente nos va a interesar, pero al no ser explícito puede ocasionar confusión a devs que no estén acostumbrados a trabajar con Vue, y además nos limita a la hora de extraer ciertas partes de lógica a archivos JavaScript.
Componentes complejos: cuando nuestros componentes crecen, la Options API provoca que partes de código que pertenecen a un concepto de nuestra lógica de negocio queden repartidas en data, computed, methods, etc. Esto hace que el código sea difícil de entender y mantener.
Reutilización de código: la Options API nos permite compartir código entre componentes usando mixins, pero, como veremos más adelante en el curso, tienen inconvenientes a nivel de lectura y mantenibilidad de código.

En los siguientes vídeos veremos cómo la Composition API nos ayudará a mitigar estos problemas.

La función setup es la que nos permitirá convertir un componente de Options API a Composition API. Esta función recibe dos parámetros: props, las propiedades que el componente recibe de su parent component, y context, un objeto que podemos desestrucurar en slots, attrs y emit. Es importante tener en cuenta que no podemos desestructurar directamente el parámetro props, ya que es un objeto reactivo y perdería su reactividad.


Para crear objetos reactivos usaremos la función reactive:


const state = reactive({
	totalCourses: 0
})

Si por el contrario queremos crear directamente referencias a valores, podemos usar la función ref. Para acceder al valor de la referencia, tendremos que hacerlo via la propiedad value (cuando usemos referencias en el template, no hará falta ya que Vue se encarga de añadirlo por nosotros):


const totalCourses = ref(0)

totalCourses.value = 10

Para desestructurar un objeto reactivo podemos usar la función toRefs, que los convertirá en referencias:


const { perPage } = toRefs(props)

La función setup va a devolver un objeto que corresponderá a las propiedades y funciones a las que el template tendrá acceso. De esta forma, podemos solo exponer lo que nos interese, y queda mucho más claro para otros devs lo que está pensado para ir al template y lo que pertenece solo a la capa de lógica.


# Vue 3: events validation

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

Puedes encontrar el código correspondiente a esta lección en GitHub.


En Vue 2 podemos definir lo que un componente recibe en forma de props y añadir información sobre si cada prop es required o no, y el tipo de dato que llegará (si es Number, String, etc). Esto, además de ser útil para ver warnings si una propiedad no es lo que se espera, es muy útil de cara a comunicarles a otros devs información sobre el componente.


En cambio, en Vue 2 no tenemos forma de especificar los eventos que ese componente va a emitir. Si otros devs llegan a nuestro componente, tendran que buscar línea por línea los eventos que ese componente va a emitir.


En Vue 3 tenemos la opción de definir los eventos de forma similar a lo que hacemos con props. Podemos simplemente especificar los nombres de los eventos:


export default defineComponent({
  emits: ['update:query'],
	methods: {
    handleSearch(ev) {
      this.$emit("update:query", ev.target.value);
    },
  },
});

De esta forma, otros devs van a ver rápidamente lo que ese componente va a emitir. Además, si nos equivocamos y por ejemplo escribimos "update:quer", la extensión Vetur (disponible para VSCode), nos avisará del error.


También podemos pasar una función para validar el valor que va a emitir el evento, para asegurarnos que por ejemplo el valor emitido es del tipo que esperamos, o que cumpla cierto formato:


export default defineComponent({
  emits: {
	  "update:query": value => {
	    return typeof value === "string";
	  },
	},
	methods: {
    handleSearch(ev) {
      this.$emit("update:query", ev.target.value);
    },
  },
});

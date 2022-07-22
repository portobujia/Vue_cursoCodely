# Vue 3: From Mixins to Composition API

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

En Vue 2, la forma más habitual de compartir código entre varios componentes era usar mixins. En Vue 3 aún podemos usarlo, pero en este vídeo veremos que la Composition API ofrece muchas ventajas y puede ser una opción mejor.


El problema de los mixins es sobretodo de comprensión de código. Cuando llegamos a un componente que usa mixins, no está claro qué hacen ni que métodos y variables de estado añaden al componente, y al no tener forma de definir qué podemos sobreescribir del mixin en el componente y qué no, podemos encontrarnos con conflictos a menudo.


Por contra, si usamos la Composition API podemos ser mucho más explícitos. Para crear hooks reutilizables vamos a crear y exportar una función. Ésta nos va a devolver lo que necesitemos usar en los componentes, por ejemplo, una función para trackear eventos:


// use/tracking.js

import { useStore } from "vuex";
import { api } from "../services/api";

export function useTracking() {
  const store = useStore();

  function trackEvent(ev: string, payload?: {}) {
    console.log(`track ${ev} event by user ${store.getters.userId}`);

    api.trackEvent({
      user: store.getters.userId,
      event: ev,
      payload,
    });
  }

  return {
    trackEvent,
  };
}

De esta forma, cuando importemos este hook en nuestro componente, queda mucho más claro lo que podemos usar:


export default defineComponent({
  setup(props) {
    const { trackEvent } = useTracking();

    function handleAdd() {
      trackEvent("course-add", { course: props.course.slug });

      api.addCourse(props.course.slug);
    }

    return {
      handleAdd,
    };
  },
});

Además, al ejecutarse dentro de la función setup, estamos dentro del contexto de un componente, por lo que podemos también utilizar cosas como la función onMounted dentro de useTracking, aunque en este caso debemos valorar si vale la pena que esto ocurra sin que sea explícito en el código del componente.


Para comparar fácilmente el antes y el después puedes consultar el diff de los commits en GitHub.

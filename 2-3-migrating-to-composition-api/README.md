# Vue 3: Migrating to Composition API

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

### Descripcion

Las funciones que teníamos en methods las podemos pasar a funciones normales dentro de setup. Es importante tener en cuenta que el setup sucede cuando el componente no se ha creado aún, por lo que no tenemos acceso al this. Esto no es problema ya que podemos acceder a las variables que hemos creado en el vídeo anterior. Para acceder a la ruta del componente, podemos usar el hook useRouter que nos proporciona la librería Vue Router, y para acceder al estado global podemos usar useState de la librería Vuex.  Si queremos que el template use las funciones que antes teníamos en methods, las añadiremos al return de setup.


setup() {
  const store = useStore();
  const route = useRoute();
	
  const courses: Ref<Course[]> = ref([]);
  const totalCourses = ref(0);
	
  async function getCourses() {
    const response = await api.getUserCourses({
      id: store.getters.userId,
      page: route.query.page,
      perPage: props.perPage.value,
    });
	
    courses.value = courses.value.concat(response.courses);
    totalCourses.value = response.total;
  }

  return {
    courses,
    totalCourses,
    getCourses,
  }
}

Computed

Para migrar propiedades computadas, podemos usar la función computed. Esta se recalculará cada vez que cambie cualquier valor reactivo que se encuentre en el callback. Por ejemplo, la propiedad computada canLoadMore se recalculará cada vez que cambie totalCourses o courses:


const courses = ref([]);
const totalCourses = ref(0);

const canLoadMore = computed(() => {
  return totalCourses.value > courses.value.length;
});

Lifecycle Hooks

Finalmente, para ejecutar lógica en ciertos momentos del ciclo de vida del componente, podemos usar onMounted y los demás hooks correspondientes, y para realizar side effects cuando cambie cualquier valor reactivo, podemos usar watch, que corresponde a la opción watch que teníamos en la Options API.


onMounted(getCourses);

watch(() => route.query, getCourses);

Finalmente, retornamos solo lo que el template necesite usar, y la función setup completa queda así:


setup(props) {
  const store = useStore();
  const route = useRoute();
  const { perPage } = toRefs(props);

  const courses: Ref<Course[]> = ref([]);
  const totalCourses = ref(0);

  const coursesInProgress = computed(() => {
    return courses.value.filter(
      course => course.progress > 0 && course.progress < 100,
    );
  });

  const canLoadMore = computed(() => {
    return totalCourses.value > courses.value.length;
  });

  async function getCourses() {
    const response = await api.getUserCourses({
      id: store.getters.userId,
      page: route.query.page,
      perPage: perPage.value,
    });

    courses.value = courses.value.concat(response.courses);
    totalCourses.value = response.total;
  }

  onMounted(getCourses);

  watch(() => route.query, getCourses);

  return {
    courses: coursesInProgress,
    canLoadMore,
  };
},

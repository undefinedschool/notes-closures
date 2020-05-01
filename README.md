> El siguiente contenido fue elaborado por [@_nhsz](https://twitter.com/_nhsz) como guía para las clases de [undefined school](https://twitter.com/undefinedSchool)  
> Son bienvenidos los _issues_ y _PRs_ para mejorar el contenido, corregir errores, etc. 

> 👉 Si te resultó útil, **se agradece que lo compartas para que le llegue a más gente!**

# Notas sobre Closures

## Scope (repaso)

Define el _alcance_ de una variable, es decir, dónde puede o no utilizarse.

Recordemos que en JavaScript, el _scope_ de una variable depende de cómo la definamos:

- `var`: define como scope a la función que contiene a la variable (y global en el caso de ser una variable suelta)
- `let` y `const`: definen un scope por bloque de código

👉 **Seimpre que sea posible, es recomendable utilizar variables globales**: podemos tener colisiones de nombres (si estamos utilizando módulos por ejemplo) y estamos generando [_side-effects_](https://github.com/undefinedschool/notes-fp-js#side-effects) que vuelven menos mantenible y más frágil a nuestro código.

## FP y Funciones

En el [paradigma funcional](https://github.com/undefinedschool/notes-fp-js), las [funciones](https://github.com/undefinedschool/notes-fp-js#funciones-puras) son el bloque fundamental que utilizamos para construir nuestras aplicaciones. Si bien vimos que tienen muchas ventajas, también tienen ciertas limitaciones. Por ejemplo, _no tienen memoria_, se olvidan de todo cada vez que retornan un valor, no tenemos acceso a un _estado global_, algo que nos resultaría muy útil.

Pero cómo podríamos tener esta funcionalidad sin caer en los problemas (side effects generados) de utilizar, por ejemplo, variables globales?

## Closures

- https://medium.com/@gloriafercu/scope-y-closure-en-javascript-9166707b95b5
- https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36

**En JavaScript, las funciones no son sólo funciones. También son closures.**

Las Closures son importantes porque permiten controlar qué queda dentro y fuera del _scope_ de una función y qué variables son compartidas entre funciones que se encuentren bajo el mismo _scope_. Entender cómo las variables y funciones se relacionan entre sí es clave para entender lo que sucede en nuestro código, tanto en el paradigma funcional como en el de objetos.

Un closure es la combinación de una función ligada a referencias a su scope léxico. Es decir, un closure permite que una función tenga acceso a un scope externo (variables, estado) definido fuera de la misma. 

**En JavaScript, los closures se crean cada vez que se crea una función.**

— Functions are our units to build with but they’re
limited - they forget everything each time they finish
running - with no global state
— Imagine if we could give our functions memories

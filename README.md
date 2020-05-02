> El siguiente contenido fue elaborado por [@_nhsz](https://twitter.com/_nhsz) como guía para las clases de [undefined school](https://twitter.com/undefinedSchool)  
> Son bienvenidos los _issues_ y _PRs_ para mejorar el contenido, corregir errores, etc. 

> 👉 Si te resultó útil, **se agradece que lo compartas para que le llegue a más gente!**

# Notas sobre Closures

## Scope (breve repaso)

Define el _alcance_ de una variable, es decir, dónde puede o no utilizarse. En JavaScript tenemos 3 tipos de scope: global, por función y por bloque.

- **global**: variables que se crean fuera de las funciones y que son accesibles desde cualquier punto.
- **por función**: variables locales a una función, que sólo pueden accederse dentro de la misma.
- **por bloque**: variables locales a un bloque, que sólo pueden accederse dentro del mismo (por ejemplo, los índices en un ciclo `for`).

El _scope_ de una variable depende de cómo la definamos:

- si utilizamos **`var`**, se define como **scope** a la **función** que contiene a la variable (y este pasa a ser global en el caso de definir una variable suelta). Además, las variables definidas con `var` tienen [_hoisting_](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting).
- si utilizamos **`let`** o **`const`**, se define un **scope por bloque de código**.

> **Siempre que sea posible, es recomendable evitar utilizar variables globales**: podemos tener colisiones de nombres (por ejemplo, si estamos utilizando módulos o importando alguna librería externa) y estamos generando [_side-effects_](https://github.com/undefinedschool/notes-fp-js#side-effects), que vuelven menos mantenible y más frágil a nuestro código.

## FP y Funciones

En el [paradigma funcional](https://github.com/undefinedschool/notes-fp-js), las [funciones](https://github.com/undefinedschool/notes-fp-js#funciones-puras) son el bloque fundamental que utilizamos para construir nuestras aplicaciones. Si bien vimos que tienen muchas ventajas, también nos encontramos con ciertas limitaciones. Por ejemplo, _no tienen memoria_: se olvidan de todo el historial de ejecución cada vez que retornan un valor y no tenemos acceso a un _estado global_, algo que nos resultaría muy útil.

Pero cómo podríamos tener esta funcionalidad sin caer en los problemas (side effects generados) de utilizar, por ejemplo, variables globales?

## Closures

[WIP]

Mientras que las funciones closure son aquellas que hacen referencia a variables externas a su ámbito o scope.

[WIP]

Las Closures son importantes porque permiten controlar qué queda dentro y fuera del _scope_ de una función y qué variables son compartidas entre funciones que se encuentren bajo el mismo _scope_. Entender cómo las variables y funciones se relacionan entre sí es clave para entender lo que sucede en nuestro código, tanto en el paradigma funcional como en el de objetos.

Un closure es la combinación de una función ligada a referencias a su scope léxico. Es decir, un closure permite que una función tenga acceso a un scope externo (variables, estado) definido fuera de la misma. 

**En JavaScript, los closures se crean cada vez que se crea una función.**

[WIP]

— Functions are our units to build with but they’re
limited - they forget everything each time they finish
running - with no global state
— Imagine if we could give our functions memories

[WIP]

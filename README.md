> El siguiente contenido fue elaborado por [@_nhsz](https://twitter.com/_nhsz) como guía para las clases de [undefined school](https://twitter.com/undefinedSchool)  
> Son bienvenidos los _issues_ y _PRs_ para mejorar el contenido, corregir errores, etc. 

> 👉 Si te resultó útil, **se agradece que lo compartas para que le llegue a más gente!**

# Notas sobre Closures

## Scope

https://www.freecodecamp.org/news/an-introduction-to-scope-in-javascript-cbd957022652/

## Closures

- https://medium.com/@gloriafercu/scope-y-closure-en-javascript-9166707b95b5
- https://www.youtube.com/watch?v=CQqwU2Ixu-U
- https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36

**En JavaScript, las funciones no son sólo funciones. También son closures.**

Las Closures son importantes porque permiten controlar qué queda dentro y fuera del _scope_ de una función y qué variables son compartidas entre funciones que se encuentren bajo el mismo _scope_. Entender cómo las variables y funciones se relacionan entre sí es clave para entender lo que sucede en nuestro código, tanto en el paradigma funcional como en el de objetos.

Un closure es la combinación de una función ligada a referencias a su scope léxico. Es decir, un closure permite que una función tenga acceso a un scope externo (variables, estado) definido fuera de la misma. 

**En JavaScript, los closures se crean cada vez que se crea una función.**

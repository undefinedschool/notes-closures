# ![Closures](https://i.imgur.com/7fbQZJE.png)

<div align="center">  
  <p align="center">
  <sub>
    Estas notas fueron elaboradas por <a href="https://twitter.com/_nhsz" target="_blank" rel="noreferrer noopener">@_nhsz</a>, como parte del contenido de los cursos de <a href="https://undefinedschool.io/" target="_blank" rel="noreferrer noopener"><strong>undefined school</strong></a>, una escuela de <strong>Desarrollo Web Full Stack JavaScript</strong>, <a href="https://github.com/undefinedschool/" target="_blank" rel="noreferrer noopener">100% Open Source</a>, con <strong>mentorías personalizadas para grupos reducidos</strong> y el foco puesto en los <strong>fundamentos</strong> y <strong>conceptos avanzados</strong> ⚡
  </sub>
  </p>
  
  <p align="center">
  <sub>
    Si te resultaron útiles, te cuento que soy muy fan del café; si querés colaborar para que no me quede dormido y siga escribiendo guías, apuntes y más <strong>contenido Open Source en español</strong>, podés invitarme uno, gracias! ❤️
  </sub>
  </p>
  
  <h4 align="center">
  ☕
  <code> 
  <a mp-mode="dftl" href="https://www.mercadopago.com.ar/checkout/v1/redirect?pref_id=243772354-b32a750f-2505-41c1-8e5e-9dcdb4536593" name="MP-payButton" class='blue-ar-l-rn-none'>
    <strong>Invitame 1 café!</strong>
  </a>
  </code>
  </h4>
  <hr>
</div>

👉 Ver [todas las notas](https://github.com/undefinedschool/notes)

## Contenido

- [Scope (breve repaso)](https://github.com/undefinedschool/notes-closures#scope-breve-repaso)
  - [Scope léxico](https://github.com/undefinedschool/notes-closures#scope-l%C3%A9xico)
- [FP y Funciones](https://github.com/undefinedschool/notes-closures#fp-y-funciones)
- [Closures](https://github.com/undefinedschool/notes-closures#closures)
  - [Closures y el contexto de ejecución](https://github.com/undefinedschool/notes-closures#closures-y-el-contexto-de-ejecuci%C3%B3n)
  - [Creando un closure](https://github.com/undefinedschool/notes-closures#creando-un-closure)
  - [Usos comunes](https://github.com/undefinedschool/notes-closures#usos-comunes)
    - [Definir variables y propiedades _privadas_](https://github.com/undefinedschool/notes-closures#definir-variables-y-propiedades-privadas)
    - [Evitar las variables globales](https://github.com/undefinedschool/notes-closures#evitar-las-variables-globales)
    - [Funciones con estado (feat. _React Hooks_)](https://github.com/undefinedschool/notes-closures#funciones-con-estado-feat-react-hooks)
    - [FP: Aplicaciones Parciales y _Currying_](https://github.com/undefinedschool/notes-closures#fp-aplicaciones-parciales-y-currying)

---

## Scope (breve repaso)

Define el _alcance_ de una variable, es decir, dónde puede o no utilizarse. En JavaScript tenemos 3 tipos de scope: global, por función y por bloque.

- **global**: variables que se crean fuera de las funciones y que son accesibles desde cualquier punto.
- **por función**: variables locales a una función, que sólo pueden accederse dentro de la misma.
- **por bloque**: variables locales a un bloque, que sólo pueden accederse dentro del mismo (por ejemplo, los índices en un ciclo `for`).

El _scope_ de una variable depende de cómo la definamos:

- si utilizamos **`var`**, se define como **scope** a la **función** que contiene a la variable (y este pasa a ser global en el caso de definir una variable suelta). Además, las variables definidas con `var` tienen [_hoisting_](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting).
- si utilizamos **`let`** o **`const`**, se define un **scope por bloque de código**.

> **Siempre que sea posible, es recomendable evitar utilizar variables globales**: podemos tener colisiones de nombres (por ejemplo, si estamos utilizando módulos o importando alguna librería externa) y estamos generando [_side-effects_](https://github.com/undefinedschool/notes-fp-js#side-effects), que vuelven menos mantenible y más frágil a nuestro código.

👉 **En JavaScript, cada función crea un nuevo _contexto local de ejecución_ (o _scope local_).**

Tener _closures_ es una feature importante porque permite controlar qué queda dentro y fuera del _scope_ de una función y qué variables son compartidas entre funciones que se encuentren bajo el mismo _scope_ (pensemos en funciones definidas dentro de otras). Entender cómo las variables y funciones se relacionan entre sí es clave para entender lo que sucede en nuestro código, tanto en el paradigma funcional como en el de objetos.

### Scope léxico

En JavaScript, las funciones tienen su propio _ámbito léxico_, lo cual significa que depende de cómo son declaradas en el código y no de cuándo o cómo se ejecutan. _Léxico_ hace referencia a dónde (en qué parte del código) fue definida la función.

Determina a qué datos (variables, estado) una función tiene acceso cuando la invocamos.

## FP y Funciones

En el [paradigma funcional](https://github.com/undefinedschool/notes-fp-js) (en adelante _FP_, por _Functional Programming_), las [funciones](https://github.com/undefinedschool/notes-fp-js#funciones-puras) son el bloque fundamental que utilizamos para construir nuestras aplicaciones. Si bien vimos que tienen muchas ventajas, también nos encontramos con ciertas limitaciones. Por ejemplo, _no tienen memoria_: se olvidan de todo el historial de ejecución cada vez que retornan un valor y no tenemos acceso a un _estado global_, algo que nos resultaría muy útil.

Pero cómo podríamos tener esta funcionalidad sin caer en los problemas (side effects generados) de utilizar, por ejemplo, variables globales?

## Closures

**Un closure es la combinación de una función y un _ambiente o estado ligado_, determinado por su _scope léxico_** (el entorno donde fue definida). Es decir, un closure permite que una función tenga acceso a un scope externo (variables, estado) definido fuera de la misma, similar a lo que ocurre con las variables globales, pero controlando los _side effects_ 😎. 

**En JavaScript, los closures se crean cada vez que se crea una función, por lo que todas las funciones definen closures** (por default, el scope ligado es el _global_). En los lenguajes que no tienen closures en cambio, las variables (estado) local sólo existen durante la ejecución de la función.

En resumen, un closure almacena el _estado_ de una función (tiene un ambiente de variables _ligado_), aún después de que la misma haya retornado. En decir, la función definida en el closure _tiene memoria_ del entorno (estado) en el que fue definida.

[![Learn Closures In 7 Minutes](https://img.youtube.com/vi/3a0I8ICR1Vg/0.jpg)](https://www.youtube.com/watch?v=3a0I8ICR1Vg)
> Ver [Learn Closures In 7 Minutes](https://www.youtube.com/watch?v=3a0I8ICR1Vg)

### Closures y el contexto de ejecución

Dijimos anteriormente que cada función creaba un nuevo _contexto de ejecución_. Esto implica que

- cada variable declarada dentro de este scope es _local_.
- el scope externo a la función no tiene acceso a las variables locales.
- el scope local _puede acceder al scope externo_, gracias a los closures.

[![Part 1: JavaScript the Hard Parts: Closure, Scope & Execution Context](https://img.youtube.com/vi/DrgnNapYs1w/0.jpg)](https://www.youtube.com/watch?v=DrgnNapYs1w)
> Ver [Part 1: JavaScript the Hard Parts: Closure, Scope & Execution Context](https://www.youtube.com/watch?v=DrgnNapYs1w)

### Creando un closure

Para definir un closure, alcanza con definir una función dentro de otra: tenemos una función que retorna una función, por lo tanto se trata de una [_Higher-Order Function_](https://github.com/undefinedschool/notes-fp-js#higher-order-functions).

En el siguiente ejemplo, la constante `name` puede ser accedida desde la función `salute()`.

```js
const salute = () => {
  const name = "Sarah";
  
  const showName = () => {
    alert(name);
  }
  
  showName();
}

salute();
```

Si le agregamos parámetros a la función externa que el closure (función interna) puede utilizar, este se vuelve mucho más versátil. Por ejemplo

```js
const sayHi = name =>
  () => `Hi, ${name}!`
```

```js
const saluteCaro = sayHi('Caro');
const saluteLeo = sayHi('Leo');
const saluteLucas = sayHi('Lucas');

saluteCaro();  // 'Hi, Caro!'
saluteLeo();   // 'Hi, Leo!'
saluteLucas(); // 'Hi, Lucas!'
```

Otro ejemplo

```js
function outerFunction(outerVariable) {
  return function innerFunction(innerVariable) {
    console.log(`Outer variable: ${outerVariable}`);
    console.log(`Inner variable: ${innerVariable}`);
  }
}
```

### Usos comunes

#### Definir variables y propiedades _privadas_

**Una aplicación común de los cloures es darle privacidad a algunas partes de la interfaz de un objeto (definir propiedades o métodos _privados_)**.

> **Esto nos va a permitir escribir código que se base más en las interfaces (_encapsulación_) que en la implementación**, resultando en aplicaciones más robustas, ya que los detalles de implementación son más propensos a cambiar con el tiempo que las interfaces. 

**Recordemos que sólo podemos acceder a variables locales estando dentro del _scope_ de la función externa/contenedora. No podemos acceder al estado desde un _scope externo_, salvo a través de los _métodos privilegiados_**. En JavaScript, cualquier método expuesto, definido dentro de un closure, es un método privilegiado.

En el siguiente ejemplo, `accountBalance` es una variable global que puede ser accedida (y modificada) externamente, algo que queremos evitar!

```js
let accountBalance = 0;

function manageBankAccount() {
  return {
    deposit(amount) {
      accountBalance += amount;
    },
    withdraw(amount) {
      // ... safety logic
      accountBalance -= amount;
    }
  };
}
```

Si utilizamos closures, limitamos el scope de la variable y por lo tanto sólo puede ser modificada a través del objeto

```js
function manageBankAccount(initialBalance) {
  let accountBalance = initialBalance;
    
  return {
    getBalance() {
      return accountBalance
    },
    deposit(amount) { 
      accountBalance += amount; 
    },
    withdraw(amount) {
      if (amount > accountBalance) return 'You cannot draw that much!';

      accountBalance -= amount;
    }
  }
};

const accountManager = manageBankAccount(0);

accountManager.deposit(1000);
accountManager.withdraw(500);
accountManager.getBalance(); // 500
```

Como vemos, los closures nos permiten _encapsular datos o comportamiento_.

#### Evitar las variables globales

[![Three Techniques for Avoiding Global Variables in JavaScript Using Closure](https://img.youtube.com/vi/SMUHorBkVsY/0.jpg)](https://www.youtube.com/watch?v=SMUHorBkVsY)
> Ver [Three Techniques for Avoiding Global Variables in JavaScript Using Closure](https://www.youtube.com/watch?v=SMUHorBkVsY)

#### Funciones con estado (feat. _React Hooks_)

[WIP]

Los objetos no son la única forma que tenemos de _encapsular datos_. También podemos utilizar closures para crear _funciones con estado_ (stateful), cuyos valores de retorno dependan de este estado interno.

#### FP: Aplicaciones Parciales y _Currying_

[WIP]

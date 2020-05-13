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
  
  <p align="center">
  ☕
  <code> 
  <a mp-mode="dftl" href="https://www.mercadopago.com.ar/checkout/v1/redirect?pref_id=243772354-b32a750f-2505-41c1-8e5e-9dcdb4536593" name="MP-payButton" class='blue-ar-l-rn-none'>
    <strong>Invitame 1 café!</strong>
  </a>
  </code>
  </p>
  <hr>
</div>

👉 Ver [todas las notas](https://github.com/undefinedschool/notes)

## Contenido

- [Intro](https://github.com/undefinedschool/notes-closures#intro)
- [Scope (breve repaso)](https://github.com/undefinedschool/notes-closures#scope-breve-repaso)
  - [Scope léxico](https://github.com/undefinedschool/notes-closures#scope-l%C3%A9xico)
- [Contexto de ejecución](https://github.com/undefinedschool/notes-closures/blob/master/README.md#contexto-de-ejecuci%C3%B3n)
- [FP y Funciones](https://github.com/undefinedschool/notes-closures#fp-y-funciones)
- [Closures](https://github.com/undefinedschool/notes-closures#closures)
  - [Closures y el contexto de ejecución](https://github.com/undefinedschool/notes-closures#closures-y-el-contexto-de-ejecuci%C3%B3n)
  - [Creando un closure](https://github.com/undefinedschool/notes-closures#creando-un-closure)
  - [Usos comunes](https://github.com/undefinedschool/notes-closures#usos-comunes)
    - [Definir variables y propiedades _privadas_](https://github.com/undefinedschool/notes-closures#definir-variables-y-propiedades-privadas)
    - [Evitar las variables globales](https://github.com/undefinedschool/notes-closures#evitar-las-variables-globales)
    - [Uso más eficiente de la memoria](https://github.com/undefinedschool/notes-closures/blob/master/README.md#uso-m%C3%A1s-eficiente-de-la-memoria)
    - [Funciones con estado (feat. _React Hooks_)](https://github.com/undefinedschool/notes-closures#funciones-con-estado-feat-react-hooks)
    - [FP: Aplicaciones Parciales y _Currying_](https://github.com/undefinedschool/notes-closures#fp-aplicaciones-parciales-y-currying)

---

## Intro

Supongamos que tenemos el siguiente código, una función `getEmployee` que crea nuevos empleados.

```js
function getEmployee(name, country) {
  return { name, country };
}
 
const employeeOne = getEmployee('Robin', 'Germany');
const employeeTwo = getEmployee('Markus', 'Canada');
 
const employees = [employeeOne, employeeTwo];
```

Agreguemos un `id` para identificar a cada empleado. Este valor debe ser asignado _internamente_, ya que una vez creado y retornado el objeto, no vamos a modificarlo.

```js
function getEmployee(name, country) {
  let employeeNumber = 1;
  return { employeeNumber, name, country };
}
 
const employeeOne = getEmployee('Robin', 'Germany');
const employeeTwo = getEmployee('Markus', 'Canada');
 
const employees = [employeeOne, employeeTwo];
```

El problema que tenemos es que estamos repitiendo siempre el mismo `id` y debería ser único. Usualmente, los ids se van incrementando en 1 y nos gustaría hacer eso con el `id` de cada nuevo empleado. Pero una vez que la función `getEmployee` retorna, no tiene forma de _trackear_ el valor actual de este `id`, para luego asignar el correspondiente. Notemos que la función es pura, sólo depende de sus inputs y no mantiene un _historial_ de su estado interno luego de ejecutarse.

Entonces, algo que se nos puede ocurrir es mover este `id` fuera de la función, para que de esta forma pueda ser accedido y actualizado luego, con cada llamada a la función `getEmployee`.

```js
let employeeNumber = 1;
 
function getEmployee(name, country) {
  return { employeeNumber: employeeNumber++, name, country };
}
 
const employeeOne = getEmployee('Robin', 'Germany');
const employeeTwo = getEmployee('Markus', 'Canada');
 
const employees = [employeeOne, employeeTwo];
```

Con este cambio, `employeeNumber` pasó de poder utilizarse sólo dentro del scope de la función a estar en el _scope global_. Esto nos puede traer algunos problemas... Ahora puede ser accedido y modificado desde cualquier parte.

```js
let employeeNumber = 1;
 
function getEmployee(name, country) {
  return { 
    employeeNumber: employeeNumber++, 
    name, 
    country 
  };
}
 
const employeeOne = getEmployee('Robin', 'Germany');
employeeNumber = 50;
const employeeTwo = getEmployee('Markus', 'Canada');
 
const employees = [employeeOne, employeeTwo];
```

Nos gustaría poder seguir trackeando el estado interno, pero sin caer en los problemas que nos genera utilizar estado global (side effects). Modifiquemos el código de la sigueinte forma

```js
function getEmployeeFactory() {
  let employeeNumber = 1;
  
  return (name, country) => ({
    employeeNumber: employeeNumber++, 
    name, 
    country 
  });
}
 
const getEmployee = getEmployeeFactory();
 
const employeeOne = getEmployee('Robin', 'Germany');
const employeeTwo = getEmployee('Markus', 'Canada');
 
const employees = [employeeOne, employeeTwo];
```

Ahora `getEmployeeFactory` pasa a ser una _HOF_ (Higher-Order Function), ya que retorna una función.

Ya no es posible modificar el `id` desde afuera del scope de la función, porque dejó de pertenecer al scope global. El `employeeNumber` pasó a ser un _estado interno_ que persiste en la función.

> **Nota:** en este ejemplo, `getEmployeeFactory()` utiliza el patrón de diseño conocido como [_Factory Pattern_](https://medium.com/@thebabscraig/javascript-design-patterns-part-1-the-factory-pattern-5f135e881192).

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

## Contexto de ejecución

El _contexto_ es toda la información necesaria para ejecutar una función, como por ejemplo las variables disnponibles para ser utilizadas (scope). Cada vez que creamos una función en JavaScript, se crea un nuevo _contexto de ejecución local_.

También existe el _contexto de ejecución global_, que se crea cuando el _engine_ de JS comienza a analizar nuestro código. Este contexto siempre está presente e incluye un [_objeto global_](https://developer.mozilla.org/en-US/docs/Glossary/Global_object) (`window` en el caso del browser, `global` si estamos en Node) y el valor al que hace referencia `this`, que en el caso del browser apunta al objeto `window`.

## FP y Funciones

En el [paradigma funcional](https://github.com/undefinedschool/notes-fp-js) (en adelante _FP_, por _Functional Programming_), las [funciones](https://github.com/undefinedschool/notes-fp-js#funciones-puras) son el bloque fundamental que utilizamos para construir nuestras aplicaciones. Si bien vimos que tienen muchas ventajas, también nos encontramos con ciertas limitaciones. Por ejemplo, _no tienen memoria_: se olvidan de todo el historial de ejecución cada vez que retornan un valor y no tenemos acceso a un _estado global_, algo que nos resultaría muy útil.

Pero cómo podríamos tener esta funcionalidad sin caer en los problemas (side effects generados) de utilizar, por ejemplo, variables globales?

## Closures

**Un closure es la combinación de una función y un _ambiente o estado ligado_, determinado por su _scope léxico_** (el entorno donde fue definida). Es decir, un closure permite que una función tenga acceso a un scope externo (variables, estado) definido fuera de la misma, similar a lo que ocurre con las variables globales, pero controlando los _side effects_ 😎. 

**En JavaScript, los closures se crean cada vez que se crea una función, por lo que todas las funciones definen closures** (por default, el scope ligado es el _global_). En los lenguajes que no tienen closures en cambio, las variables (estado) local sólo existen durante la ejecución de la función.

Un closure almacena el _estado_ de una función (tiene un ambiente de variables _ligado_), aún después de que la misma haya retornado. En decir, la función definida en el closure _tiene memoria_ del entorno (estado) en el que fue definida.

[![Learn Closures In 7 Minutes](https://img.youtube.com/vi/3a0I8ICR1Vg/0.jpg)](https://www.youtube.com/watch?v=3a0I8ICR1Vg)
> Ver [Learn Closures In 7 Minutes](https://www.youtube.com/watch?v=3a0I8ICR1Vg)

👉 Por lo tanto, un closure termina siendo un tipo especial de objeto que combina lo siguiente:

- una función
- el entorno en el cual la función fue definida ([scope léxico](https://github.com/undefinedschool/notes-closures#scope-l%C3%A9xico))
  - el entorno consiste de cualquier variable local o función definida dentro de este scope en el momento en el que se define el closure.

### Closures y el contexto de ejecución

Dijimos anteriormente que cada función creaba un nuevo _contexto de ejecución_. Esto implica que

- cada variable declarada dentro de este scope es _local_.
- el scope externo a la función no tiene acceso a las variables locales.
- el scope local _puede acceder al scope externo_, gracias a los closures.

[![Part 1: JavaScript the Hard Parts: Closure, Scope & Execution Context](https://img.youtube.com/vi/DrgnNapYs1w/0.jpg)](https://www.youtube.com/watch?v=DrgnNapYs1w)
> Ver [Part 1: JavaScript the Hard Parts: Closure, Scope & Execution Context](https://www.youtube.com/watch?v=DrgnNapYs1w)

Al crear un closure, [se enlaza una función a un entorno de variables](https://javascript.info/closure#lexical-environment) (determinado por su scope léxico). 

Una función definida dentro de otra y luego retornada, mantiene el acceso a este entorno a través de una propiedad oculta `[[scope]]` (recordemos que [las funciones son objetos!](https://github.com/undefinedschool/notes-functions-first-class)) que persiste aún cuando la función es retornada. De esta forma, la función retornada (closure) va a buscar las referencias a variables y otros objetos primero en su scope local y luego en el entorno ligado, antes de pasar a buscar en el scope global.

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

#### Uso más eficiente de la memoria

Una ventaja indirecta de evitar las variables globales utilizando closures, es que _evitamos la polución del scope global_, teniendo menos funciones, variables y valores viviendo continuamente en la memoria global de nuestro programa, resultando en una aplicación más eficiente.

Por ejemplo, cada vez que invocamos `produceChocolate` estamos creando y destruyendo continuamente el array `chocolateFactory`, porque pertenece al scope local de la función.

```js
const produceChocolate = index => {  
  const chocolateFactory = new Array(7000).fill('chocolate');
 
  return chocolateFactory[index];
}

produceChocolate(1);
produceChocolate(996);
produceChocolate(6784);
```

En cambio, podemos utilizar closures, para crear el array sólo 1 vez.

```js
const produceChocolateEfficiently = () => {
  const chocolateFactory = new Array(7000).fill('chocolate');
  
  // chocolateFactory is stored in the closure memory, 
  // because it has reference in execution scope of inner function below
  return (index) => chocolateFactory[index];
}

const getProduceChocolateEfficiently = produceChocolateEfficiently();

getProduceChocolateEfficiently(1243);
getProduceChocolateEfficiently(6832);
getProduceChocolateEfficiently(345);
```

#### Funciones con estado (feat. _React Hooks_)

[WIP]

Los objetos no son la única forma que tenemos de _encapsular datos_. También podemos utilizar closures para crear _funciones con estado_ (stateful), cuyos valores de retorno dependan de este estado interno.

#### FP: Aplicaciones Parciales y _Currying_

[WIP]

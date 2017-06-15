Traducido de https://ponyfoo.com/articles/es6-destructuring-in-depth

# Desestructuraci�n de ES6 en profundidad

### _**Mencion� brevemente algunas caracter�sticas de ES6 (y c�mo comenzar con Babel) en las series de art�culos de React que estuve escribiendo, y ahora quiero enfocarme en las caracter�sticas mismas del lenguaje. Le� una tonelada de cosas sobre ES6 y ES7 y es hora de que empecemos a debatir caracter�sticas de ES6 y ES7 aqu� en Pony Foo.**_

Este art�culo advierte **sobre exaltarse demasiado** con las caracter�sticas del lenguaje ES6. Luego comenzaremos la serie con la desesestructuraci�n de ES6, y cu�ndo es m�s �til, as� tambi�n como sus trampas y algunas advertencias.
 
## Un consejo
 
Cuando est�s en duda, probablemente debas volver por defecto a ES5 o sintaxis anteriores en vez de adoptar ES6 s�lo porque puedas. Con esto no quiero decir que usar sintaxis ES6 sea una mala idea - al contrario, como ver�n �estoy escribiendo un art�culo de ES6! Mi preocupaci�n es que cuando adoptemos las caracter�sticas de ES6, lo hagamos **s�lo si mejoran en absoluto la calidad de nuestro c�digo** y no s�lo por el _"factor cool"_ - lo que sea que signifique eso.
 
El enfoque que estuve tomando hasta ahora es escribir cosas en puro ES5 y luego agregar un poco de az�car ES6 encima, donde genuinamente mejorar�a mi c�digo. Supongo que con el tiempo, podr� identificar m�s r�pidamente contextos donde una caracter�stica de ES6 valga la pena usarse sobre ES5. Pero cuando empieces, ser�a una buena idea -no- entusiasmarse demasiado pronto. En vez de eso, analizar con cuidado que encajar�a mejor en tu c�digo y **ser consciente al adoptar ES6.**
 
De esta forma, **aprender�s a usar** las nuevas caracter�sticas a tu favor en vez de solo aprender la sintaxis.
 
�Ahora pasemos a lo cool!
 
 
# Desestructuraci�n
 
Esta es, f�cilmente, una de las caracter�sticas que m�s estuve usando. Tambi�n es una de las m�s simples. Une propiedades a tantas variables como necesitas y funciona con colecciones (arrays) y objetos.

```javascript 
var foo = { bar: 'pony', baz: 3 }
var {bar, baz} = foo
console.log(bar)
// <- 'pony'
console.log(baz)
// <- 3
``` 
 
Hace que sea muy r�pido quitar una propiedad espec�fica de un objeto. Tambi�n puedes corresponder propiedades con alias.
```javascript 
var foo = { bar: 'pony', baz: 3 }
var {bar: a, baz: b} = foo
console.log(a)
``` 
Tambi�n puedes obtener propiedades en otros niveles tanto como quieras, y tambi�n podr�as darle un alias a esos enlaces profundos. 
```javascript 
var foo = { bar: { deep: 'pony', dangerouslySetInnerHTML: 'lol' } }
var {bar: { deep, dangerouslySetInnerHTML: sure }} = foo
console.log(deep)
// <- 'pony'
console.log(sure)
// <- 'lol'
// <- 'pony'
console.log(b)
// <- 3
``` 
Por defecto, propiedades no encontradas ser�n `undefined`, al igual que cuando accedes a propiedades de un objeto con el punto o la llave.
```javascript 
var {foo} = {bar: 'baz'}
console.log(foo)
// <- undefined
``` 
Si intentas acceder a una propiedad anidada profundamente de una clase padre que no existe, entonces obtendr�s una excepci�n.
```javascript 
var {foo:{bar}} = {baz: 'ouch'}
// <- Exception
``` 
Eso tiene mucho sentido si piensas que desestructurar es como el az�car del ES5, como en el c�digo debajo.
```javascript 
var _temp = { baz: 'ouch' }
var bar = _temp.foo.bar
// <- Exception
``` 
Una buena propiedad de desestructurar es que permite que intercambies variables sin la necesidad de la infame variable `aux`.
```javascript 
function es5 () {
  var left = 10
  var right = 20
  var aux
  if (right > left) {
    aux = right
    right = left
    left = aux
  }
}
```
```javascript
function es6 () {
  var left = 10
  var right = 20
  if (right > left) {
    [left, right] = [right, left]
  }
}
``` 
Otro aspecto c�modo de desestructurar es la habilidad de obtener claves cuando usas [nombres de propiedades computadas](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names).
```javascript 
var key = 'such_dynamic'
var { [key]: foo } = { such_dynamic: 'bar' }
console.log(foo)
// <- 'bar'
``` 
En ES5, desde tu lado, llevar�a una declaraci�n extra y una asignaci�n de variable.
```javascript 
var key = 'such_dynamic'
var baz = { such_dynamic: 'bar' }
var foo = baz[key]
console.log(foo)
``` 
Tambi�n puedes definir valores por defecto, para el caso donde la propiedad extra�da eval�a a `undefined`.
```javascript 
var {foo=3} = { foo: 2 }
console.log(foo)
// <- 2
var {foo=3} = { foo: undefined }
console.log(foo)
// <- 3
var {foo=3} = { bar: 2 }
console.log(foo)
// <- 3
```

Desestructurar funciona tambi�n para las colecciones _(arrays)_, como mencion� antes. Nota como ahora estoy **usando corchetes** del lado de la desestructuraci�n de la declaraci�n.
```javascript  
var [a] = [10]
console.log(a)
// <- 10
```
Aqu�, de nuevo, podemos usar los valores por defecto y seguir las mismas reglas.
```javascript  
var [a] = []
console.log(a)
// <- undefined
var [b=10] = [undefined]
console.log(b)
// <- 10
var [c=10] = []
console.log(c)
// <- 10
``` 
Cuando se trata de colecciones _(arrays)_, puedes saltear los elementos que no te interesan.
```javascript  
var [,,a,b] = [1,2,3,4,5]
console.log(a)
// <- 3
console.log(b)
// <- 4
```
Tambi�n puedes usar desestructuraci�n en la lista de par�metros en una `function`.

```javascript  
function greet ({ age, name:greeting='she' }) {
  console.log(`${greeting} is ${age} years old.`)
}
greet({ name: 'nico', age: 27 })
// <- 'nico is 27 years old'
greet({ age: 24 })
// <- 'she is 24 years old'
```  
 
En t�rminos generales, as� es **c�mo puedes** usar desestructuraci�n. Pero �**para qu�** es buena la desestructuraci�n?
 
# Casos de uso de desustructuraci�n
 
Existen muchas situaciones donde desestructurar puede ser �til. Aqu� est�n alguna de las m�s comunes. Cuando sea que tengas un m�todo que devuelve un objeto, desestructurar hace que sea m�s conciso de interactuar con �l.
```javascript  
function getCoords () {
  return {
    x: 10,
    y: 22
  }
}
var {x, y} = getCoords()
console.log(x)
// <- 10
console.log(y)
// <- 22
``` 
Un caso similar de uso pero en la posici�n opuesta, es poder definir opciones por defecto cuando tienes un m�todo con muchas opciones que necesitan valores por defecto.  Particularmente, es interesante como una alternativa para nombrar par�metros en otros lenguajes como Python y C#.
```javascript  
function random ({ min=1, max=300 }) {
  return Math.floor(Math.random() * (max - min)) + min
}
console.log(random({}))
// <- 174
console.log(random({max: 24}))
// <- 18
```
Si quer�as hacer que las opciones del objeto sean **completamente opcionales**, podr�as cambiar la sintaxis a lo siguiente:
```javascript  
function random ({ min=1, max=300 } = {}) {
  return Math.floor(Math.random() * (max - min)) + min
}
console.log(random())
// <- 133
``` 
Algo que encajar�a perfecto para desestructurar son las cosas como las expresiones regulares, en donde te encantar�a nombrar propiedades sin tener que recurrir a n�meros �ndices. Aqu� hay un ejemplo analizando con un `RegExp` aleatorio que [encontr� en StackOverflow](https://stackoverflow.com/questions/27745/getting-parts-of-a-url-regex/27755#27755).
```javascript  
function getUrlParts (url) {
  var magic = /^(https?):\/\/(ponyfoo\.com)(\/articles\/([a-z0-9-]+))$/
  return magic.exec(url)
}
var parts = getUrlParts('http://ponyfoo.com/articles/es6-destructuring-in-depth')
var [,protocol,host,pathname,slug] = parts
console.log(protocol)
// <- 'http'
console.log(host)
// <- 'ponyfoo.com'
console.log(pathname)
// <- '/articles/es6-destructuring-in-depth'
console.log(slug)
// <- 'es6-destructuring-in-depth'
```
 
#cCaso especial: declaraciones `import`
 
Aunque las declaraciones `import` no siguen las reglas de desestructuraci�n, se comportan un poco similares. Este es probablemente el caso de uso _�casi-desestructuraci�n�_ que utilizo la mayor�a de las veces, aunque no sea desestructuraci�n en realidad. Cuando est�s escribiendo declaraciones de m�dulo `import`, puedes llamar lo que necesites de un m�dulo de API p�blico. Un ejemplo usando [`contra`](https://github.com/bevacqua/contra):
```javascript  
import {series, concurrent, map } from 'contra'
series(tasks, done)
concurrent(tasks, done)
map(items, mapper, done)
```   
Observa que, sin embargo, las declaraciones `import` tiene una sintaxis diferente. Cuando lo comparas con la desestructuraci�n, no funcionar�n ninguna de las siguientes declaraciones import:
 
* Usar valores por defecto tales como `import {series = noop} from �contra�`
* Desestructuraci�n �profunda� como `import {map: { series }} from �contra�`
* Sintaxis de alias `import {map: mapAsync} from �contra�`
 
 
La raz�n principal de estas limitaciones es que la declaraci�n `import` trae un _enlace_ y no una referencia o valor. Esta es una diferenciaci�n importante que exploraremos m�s en profundidad en un futuro art�culo acerca de los m�dulos de ES6.

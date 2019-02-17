# Aplicando código limpio y buenas prácticas

Notas del vídeo [JavaScript kata: aplicando clean code y buenas prácticas en vivo](https://www.youtube.com/watch?v=C5IrXwu6nSQ)

## SOLID

- **S**ingle responsability principle: un objeto tiene que tener un único motivo para cambiar.

- **O**pen/close principle: una entidad debe ser abierta para su extensión pero cerrada para su modificación.

- **L**iskov sustitution principle: en una jerarquía um hijo B tiene que ser intercambiable por su clase base.

- **I**nterface segregation principle: un cliente no debe ser forzado a implementar métodos que no usará.

- **D**ependency inversion principle: un cliente debe depender de abstracciones.


## Nombres

- Deben ser claros en su intención.

- Deben indicar responsabilidad única.

- No deben tener prefijos o sufijos.

- Las variables booleanas deben responder preguntas positivas.


### Olores

- And, Or, If (`IfUserIsValid`, `CreateTagsAndSetTooltips`, etc). Indica que hace demasiadas cosas.

- El código tiene efectos secundarios. Por ejemplo una función que se llama `AltaUsuario` y por detrás hace más cosas como borrar una fila en una tabla.

- Tipos de datos en el nombre. Ej: `intContador`, `bolEstoyAbierto`, `strNombre`, etc.

- Booleanos que no responden una pregunta.

- Funciones con efectos secundarios no descritos por su nombre.

---

### Principios que rompe

- Aumenta la carga cognitiva del código. 

- No deja clara la intención del código.

- Principio de responsabilidad única (la **S** de **S**OLID).

- [KISS](https://es.wikipedia.org/wiki/Principio_KISS).

---

### Soluciones

- Verbalizar con un compañero (o con [un pato de goma](https://es.wikipedia.org/wiki/M%C3%A9todo_de_depuraci%C3%B3n_del_patito_de_goma)).

- Refactorizar hasta que el nombre sea limpio.

---


## Condicionales

- Comparar explícitamente.

- Usar condicionales positivos (evitar doble negación). Ej:

```javascript
if (!isNotValid()) { ... }
```

- Asignar implícitamente.

- Evitar números mágicos.

- Evitar condicionales complejos.

### Olores

- Números mágicos.

- Asignaciones de booleanos en condicionales.

- Comentarios sobre una condición.

- Condiciones difíciles de comprender.

---

### Principios que rompe

- [DRY](https://es.wikipedia.org/wiki/No_te_repitas).

- Aumenta la carga cognitiva del código.

---

### Soluciones

- Siempre usar booleanos que respondan a preguntas positivas.

- Poner números mágicos en constantes.

- Crear funciones si la condición es una regla de negocio.

- Utilizar variables intermedias.

---


## Funciones

- Evitar el "código flecha" (el código tiene forma de punta de flecha).

- Evitar funciones con demasiada responsabilidad.

- Evitar funciones con efectos secundarios.

- Evitar muchos argumentos.

- Evitar funciones con argumentos de tipo *flag*.

Ejemplo de código flecha:

```javascript
    if (...) {
        if (...) {
            ...
        } else {
            if (...) {
                if (...) {
                    ....
                } else if (...) {
                    ...
                }
            } else {
                ...
            }
        }
    }
```

### Olores

- Hay que hacer avanzar de página para leerlas, tanto vertical como horizontalmente.

- Tienen demasiados parámetros.

- Intención poco clara.

- Cambian con mucha frecuencia.


### Soluciones

- [Clausulas de guarda](https://refactoring.com/catalog/replaceNestedConditionalWithGuardClauses.html).

- Fallar rápido. Lo mismo que el punto anterior pero lanzando excepciones.

- Extraer a un método.

- Crear nuevas clases.

- Renombrar.


## Comentarios

- Pueden indicar que nuestro código no es claro.

- Evitar comentarios obvios.

- Evitar comentarios desactualizados.


### Olores

- Los comentarios son más grandes que el código.

- El comentario dice lo mismo que el nombre de la función.

- El comentario intenta explicar algo que el código debería.

- El comentario indica miedo (Ej: `// no tocar`).

---

### Soluciones

- Al escribir un comentario pensar por qué lo hacemos.

- Cambiar el nombre de la función/variable para que indique su intención.

- Borrar comentarios en condiciones y extraer la condición.

---


## Refactorizar

- Cambiar el código sin modificar su comportamiento (con tests).

- Hacerlo más legible.

- Lo detectamos gracias a los olores, patrones, DRY, etc.


### Aplicar algunos de los conceptos

- [KISS](https://es.wikipedia.org/wiki/Principio_KISS).

- [DRY](https://es.wikipedia.org/wiki/No_te_repitas).

- [SOLID](https://es.wikipedia.org/wiki/SOLID).

- [YAGNI](https://es.wikipedia.org/wiki/YAGNI).

- Refactorizar en pasos pequeños.

- La mejor refactorización es la que quita código (sin caer en código complejo).
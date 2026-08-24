Versión final (la más agresiva) del programa: cada instancia que ataca una subred de tamaño `n` se replica en **`a` copias**, cada una encargada de una subred de tamaño `n/b`. Además, antes de dividirse, realiza una tarea de coordinación entre las copias cuyo costo está dado por una función `f(n)`.

a) Plantear la relación de recurrencia `T(n)` que modela este escenario, identificando claramente quiénes son `a`, `b` y `f(n)` (van a estar dados en el enunciado completo que te entregue el/la docente, o podés proponer vos mismo/a valores concretos si tu docente te lo indica).

b) Resolver la recurrencia usando el **Teorema Maestro**, indicando explícitamente:
   - En qué caso del teorema cae.
   - Por qué se cumplen (o no) las condiciones de ese caso.

c) Comparando con lo que armaste en el Ejercicio 2, explicá con tus palabras qué representa, en el árbol de recursión, el caso del Teorema Maestro en el que cayó este ejercicio (¿domina el costo de las hojas? ¿el costo de la raíz? ¿están equilibrados?).

d) *(Desafío opcional)* ¿Qué harías si `f(n)` no fuera una función polinomial, o si no se cumpliera la condición de regularidad del Teorema Maestro? ¿A qué herramienta de las vistas en el sendero volverías?
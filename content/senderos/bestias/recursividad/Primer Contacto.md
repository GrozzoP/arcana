Encontraste la rutina de replicación más primitiva del programa, que corre sobre una lista de `n` equipos a infectar:

```
function REPLICAR(equipos, n):
    if n <= 1:
        infectar(equipos[0])
        return
    infectar(equipos[0])
    REPLICAR(equipos[1:n], n-1)
```

a) Trazar la ejecución de `REPLICAR` para `n = 4`, identificando claramente el caso base, la pila de llamadas y el orden en que se resuelven.

b) A partir del código, plantear la relación de recurrencia `T(n)` que describe el costo de `REPLICAR` en función de `n` (no hace falta resolverla).
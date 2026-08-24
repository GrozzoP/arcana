El programa evolucionó. En esta nueva versión, cada instancia que infecta una subred de tamaño `n`:

1. Ejecuta una rutina de camuflaje sobre toda la subred (costo lineal en `n`), para evitar ser detectada.
2. Se divide en **2 copias**, y cada una ataca **una mitad** de la subred restante.

a) Plantear la relación de recurrencia `T(n)` que modela el costo total del ataque.

b) Resolver la recurrencia por **telescopía**, mostrando el desarrollo paso a paso.

c) Verificar el resultado construyendo el **árbol de recursión** correspondiente (niveles, costo por nivel, cantidad de niveles y costo total).

d) ¿Los dos métodos te dieron el mismo orden de crecimiento? Justificá brevemente por qué tiene sentido que así sea.
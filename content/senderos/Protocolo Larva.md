**Temática:** Recursividad · Relaciones de Recurrencia · Teorema Maestro

## Contexto

Sos analista de seguridad en un centro de datos. Detectaste un programa autorreplicante que se propaga por la red: cada "generación" genera copias de sí misma para infectar más equipos. A medida que avanza tu investigación, encontrás versiones cada vez más sofisticadas de su algoritmo de replicación, y necesitás entender cuánto trabajo hace el sistema en cada etapa para poder anticipar cuándo va a colapsar la red.

![[Primer Contacto]]

---

![[Mutación]]

---

![[Enjambre]]

---

## Conclusión del sendero

A lo largo de las tres etapas de "Protocolo Larva" recorriste el mismo camino que suele aparecer al analizar el costo de un algoritmo recursivo:

1. **Primer contacto** te obligó a *traducir* código recursivo en una relación de recurrencia: el paso bisagra sin el cual no se puede aplicar ninguna técnica de resolución.
2. **Mutación** te mostró que telescopía y árbol de recursión son dos caminos distintos hacia el mismo resultado: uno más algebraico, otro más visual/intuitivo.
3. **Enjambre** te presentó el Teorema Maestro como un atajo para resolver rápido una familia amplia de recurrencias: pero un atajo que solo tiene sentido si entendés *por qué* funciona, es decir, si podés conectarlo con lo que ya sabés del árbol de recursión.

La idea de fondo: el Teorema Maestro no reemplaza al árbol de recursión, lo resume. Cuando el teorema no se pueda aplicar (recurrencias no cubiertas por sus hipótesis), telescopía y árbol siguen siendo tus herramientas de resolución generales.

---
title: Skip List
tags:
  - data-structures
alias:
  - alias
---
## 1. Qué es y cómo funciona

### Intuición


### Definición y propiedades


### Representación


## 2. Operaciones y complejidad

### Operaciones principales
- Item

### Complejidad
- Item

### Detalles operativos


## 3. Implementación

### Idea de implementación
- Item

### Invariantes
- Item

### Ejemplo de código

```python

```

```python

```

```python

```

## 4. Uso y criterio

### Casos de uso

- Acceder o actualizar datos de manera concurrente _(por ejemplo, Redis utiliza esta estructura de datos)._
- Búsqueda de un rango de valores en un conjunto ordenado.
- Trabajar con índices en memoria _(Memtables)_ para guardar datos en memoria antes de grabarlos en disco.
- Cuando preferís el equilibrio probabilístico _(azar)_ en lugar de del reequilibrio determinístico _(en cada operación, se corrige y se verifica para que cumpla con ciertas reglas, como en un AVL-Tree o Red-Black-Tree)_.
- Si se requiere una estructura fácil de versionar, sin presenciar reestructuraciones como las rotaciones de un árbol.

### Cuándo NO usarlo

- Con una memoria limitada o pocos registros, debido a que se requieren muchos punteros extras.
- Si la cantidad de datos son aptos para estar en un array, y se realizan pocas inserciones.
- Necesitas garantías acerca del peor caso, porque esta estructura no ofrece límites exactos para esta medida de complejidad.
- Cuando se precisan estructuras de datos compactas en disco.
- En sistemas de tiempo real estrictos, donde no es tolerable que una operación tarde más de lo esperado, la skip list, al ser probabilística, no puede garantizar eso.

### Comparaciones
| Estructura       | Ventaja frente a Skip List                                      | Desventaja frente a Skip List                       |
| ---------------- | ----------------------------------------------------------- | ----------------------------------------------- |
| Lista enlazada            | Menor uso de memoria _(un puntero por nodo)_               | La búsqueda es O(n), sin niveles para saltar                |
| Array   | Menor uso de memoria y acceso por índice                          | Inserciones/eliminaciones cuestan O(n)                           |
| Árbol balanceado | Garantiza O(log n) en el peor caso, no solo en promedio | Más complejo de implementar y de utilizarlo de forma concurrente       |
| Tabla de hash          | Acceso promedio O(1), más rápido para búsqueda puntual         | No mantiene orden ni permite recorridos por rango                   |

### Ventajas / desventajas

Las ventajas son:
- Implementación simple, sin rotaciones ni lógica de rebalanceo, y es fácil constituir la concurrencia de forma segur.
- Mantiene los elementos ordenados y permite recorridos por rango eficientes.
- Se adapta bien a las inserciones y eliminaciones sin necesidad de reequilibrarse _(más fácil de razonar que en un AVL-Tree o Red-Black-Tree)_.
- Complejidad temporal esperada es O(log n) para búsqueda, inserción y eliminación.

Por otro lado, las desventajas son las siguientes:
- No es determinista, porque el peor caso de las operaciones podría degradarse a O(n) _(todos los nodos quedan en un mismo nivel)_.
- Mayor uso de memoria por nodo _(en promedio el doble que una lista enlazada simple)_.
- No es apta para sistemas de tiempo real estrictos que requieren garantías absolutas de peor caso.
- Son unidireccionales, no pueden recorrer hacia atrás.

### Señales de reconocimiento
Existen varias pistas o situaciones específicas donde puede concluirse que esta estructura de datos es la adecuada:
- "Necesito una alternativa más simple que un árbol balanceado"
- "Hay múltiples hilos/procesos leyendo y escribiendo al mismo tiempo"
- "El orden importa, pero no se necesita garantía absoluta de peor caso"
- "Queremos un conjunto ordenado por puntaje o ranking"
- "Se necesita consultar todos los elementos dentro de un rango"

## 5. Relaciones y extensiones

### Variantes

- Item

### Relación con otras estructuras


### Notas avanzadas


## 6. Referencias y recursos

- Item


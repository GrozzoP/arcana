---
title: Skip List
tags:
  - data-structures
alias:
  - Lista por Saltos
---
## 1. Qué es y cómo funciona

### Intuición

Una **Lista por Saltos** (Skip List) es una [[linked list|lista enlazada]] con punteros adicionales que te permiten saltarte algunos elementos y acceder más rápidamente a la información.

Esta estructura soluciona dos problemas principales, evita recorrer todo de forma secuencial como en una lista común y logra una velocidad de búsqueda parecida a la de un árbol balanceado, pero sin rebalancear la estructura cada vez que insertás o borrás algo.
### Definición y propiedades

Se trata de una estructura de datos probabilística y jerárquica. Sus principales reglas e invariantes son:

- **Capa base completa:** El nivel inferior (Nivel 0) es una lista enlazada que contiene todos los elementos insertados, ordenados de menor a mayor.

- **Balanceo probabilístico:** La estructura no realiza un balanceo estricto. En su lugar, utiliza un generador de números aleatorios para determinar en qué niveles se encuentra cada elemento.

- **Subconjuntos anidados:** Cada nivel superior contiene un subconjunto de los elementos del nivel inmediatamente inferior.

- **Límites:** La cantidad de niveles está limitada por un valor máximo denominado `MaxLevel`. Además, todos los niveles finalizan en un nodo especial denominado `NIL`.

- **Complejidad esperada:** La distribución aleatoria de los punteros adicionales permite realizar búsquedas, inserciones y eliminaciones con una complejidad esperada de **O(log n)**.
### Representación

Internamente, la estructura es gestionada por un nodo cabecera (header) sin valor de datos, el cual contiene punteros iniciales para todos los niveles posibles.

![Representacion visual skip list](skipList.svg)

A continuación, se muestra un ejemplo del recorrido para hacer una búsqueda.

![Representacion visual skip list busqueda](skipListBusqueda.svg)

## 2. Operaciones y complejidad

### 2.1 Operaciones principales

Las operaciones básicas son:

- **`find(k)`**: localiza un elemento a partir de su clave.
- **`insert(k)`**: agrega una clave en la posición que mantiene el orden.
- **`delete(k)`**: elimina una clave de todos los niveles en los que participa.

Al conservar las claves ordenadas, también facilita:

- **`min()` / `max()`**: devuelven el primer y el último elemento.
- **`predecessor(k)` / `successor(k)`**: obtienen los elementos inmediatamente anterior y posterior a una clave.
- **`range(a, b)`**: devuelve los elementos comprendidos en un intervalo.
- **`iterate()`**: recorre todos los elementos en orden siguiendo el nivel inferior.

Como operaciones especiales:

- **`append(k)`**: agrega una clave al final, siempre que sea mayor que todas las existentes.
- **`concat(A, B)`**: une directamente dos skip lists si todas las claves de `A` son menores que las de `B`. Si sus rangos se intercalan, se realiza un merge.

### 2.2 Complejidad temporal y espacial

| Operación | Tiempo esperado | Peor caso |
|---|---:|---:|
| `find`, `insert`, `delete` | `O(log n)` | `O(n)` |
| `range`, con `k` resultados | `O(log n + k)` | `O(n)` |
| `min` | `O(1)` | `O(1)` |
| `max` | `O(log n)` | `O(n)` |
| `predecessor`, `successor` | `O(log n)` | `O(n)` |
| `iterate` | `O(n)` | `O(n)` |
| `append` | `O(log n)` | `O(n)` |
| `concat`, con rangos separados | `O(log n)` | `O(MaxLevel)` |

Los costos logarítmicos son **esperados** porque dependen de la distribución aleatoria de las alturas. El caso extremo ocurre cuando todos los nodos tienen altura 1 y ninguno participa en niveles superiores.

El análisis amortizado no es central, ya que no existen operaciones caras periódicas que deban distribuirse entre varias operaciones, como el redimensionamiento de un `ArrayList` o el *rehashing* de una tabla hash.

El espacio total es `O(n)` esperado, porque cada nodo almacena una cantidad promedio constante de enlaces. En el peor caso es `O(n · MaxLevel)`.

### 2.3 Detalles operativos

- La skip list no establece por sí sola si admite claves repetidas. La implementación debe decidir si se comporta como un conjunto, rechazando duplicados, o como una colección que permite varias apariciones de una misma clave. Si se almacenaran pares clave-valor, también debería decidirse si una clave repetida reemplaza el valor anterior.
- `insert` y `delete` suelen utilizar un arreglo temporal con los predecesores de cada nivel, lo que permite actualizar los enlaces sin volver hacia atrás. Su implementación se desarrolla en la sección 3.
- `max` puede reducirse a `O(1)` guardando una referencia al último nodo.
- Si los rangos se intercalan, `concat` sería un merge de `O(n + m)`.

## 3. Implementación

### Idea de implementación

Un Skip List es una lista enlazada ordenada con niveles extra de "atajos". Cada nodo tiene un valor `key` y un arreglo `forward[]` con un puntero por cada nivel en el que participa. La lista en sí misma mantiene un `level` (el nivel más alto actualmente en uso) y un nodo `head` sentinela con punteros a todos los niveles posibles. Además, define dos parámetros fijos: `MAX_LEVEL`, el tope que ningún nodo puede superar, y `P`, la probabilidad que gobierna cuántos niveles alcanza cada nodo nuevo.

- Buscar una clave se hace de arriba hacia abajo: en el nivel más alto se avanza mientras el siguiente nodo sea menor a la clave buscada; cuando no se puede avanzar más, se baja un nivel. Al llegar al nivel 0, el siguiente nodo es el candidato.

- Para `insert` se hace ese mismo recorrido, pero guardando en un arreglo `update[]` el último nodo visitado en cada nivel. Después se sortea el nivel del nuevo nodo mediante un proceso aleatorio: se sube un nivel con probabilidad `P` en cada paso, hasta un tope `MAX_LEVEL` (y si ese nivel supera al `level` actual de la lista, este se actualiza). Una vez determinado el nivel, se enlaza el nuevo nodo en cada uno de sus niveles usando `update[]`.

- `delete` hace el mismo recorrido para obtener `update[]`, y si el nodo existe, lo desenlaza en cada nivel donde aparecía, ajustando `forward` de cada `update[i]`.


### Invariantes

- Los niveles están anidados: todo nodo presente en el nivel *i* también está en el nivel *i-1* (y así hasta el nivel 0, donde están todos los elementos).
- Cada nivel mantiene los elementos ordenados por clave.
- El nivel de un nodo se fija al insertarlo (no cambia salvo que se borre y reinserte).
- `head` siempre existe y tiene punteros válidos (o `None`) en los `MAX_LEVEL` niveles.
- El nivel "activo" de la lista (`self.level`) nunca supera `MAX_LEVEL`, y solo crece cuando un nodo insertado sortea un nivel mayor al actual.

### Ejemplo de código

```python
import random

MAX_LEVEL = 16
P = 0.5

class Node:
    def __init__(self, key, level):
        self.key = key
        self.forward = [None] * (level + 1)

class SkipList:
    def __init__(self):
        self.head = Node(None, MAX_LEVEL)
        self.level = 0

    def _random_level(self):
        lvl = 0
        while random.random() < P and lvl < MAX_LEVEL:
            lvl += 1
        return lvl

    def search(self, key):
        node = self.head
        for i in range(self.level, -1, -1):
            while node.forward[i] and node.forward[i].key < key:
                node = node.forward[i]
        node = node.forward[0]
        return node is not None and node.key == key

    def insert(self, key):
        update = [None] * (MAX_LEVEL + 1)
        node = self.head
        for i in range(self.level, -1, -1):
            while node.forward[i] and node.forward[i].key < key:
                node = node.forward[i]
            update[i] = node

        new_level = self._random_level()
        if new_level > self.level:
            for i in range(self.level + 1, new_level + 1):
                update[i] = self.head
            self.level = new_level

        new_node = Node(key, new_level)
        for i in range(new_level + 1):
            new_node.forward[i] = update[i].forward[i]
            update[i].forward[i] = new_node

    def delete(self, key):
        update = [None] * (MAX_LEVEL + 1)
        node = self.head
        for i in range(self.level, -1, -1):
            while node.forward[i] and node.forward[i].key < key:
                node = node.forward[i]
            update[i] = node

        target = node.forward[0]
        if target is None or target.key != key:
            return  # no existe
        for i in range(self.level + 1):
            if update[i].forward[i] != target:
                break
            update[i].forward[i] = target.forward[i]
```

#### Ejemplo de uso

```python
sl = SkipList()
for k in [3, 6, 7, 9, 12, 19]:
    sl.insert(k)

print(sl.search(9))   # True
sl.delete(9)
print(sl.search(9))   # False
```

## 4. Uso y criterio

### Casos de uso

- Acceder o actualizar datos de manera concurrente _(por ejemplo, Redis utiliza esta estructura de datos)._
- Búsqueda de un rango de valores en un conjunto ordenado.
- Trabajar con índices en memoria _(Memtables)_ para guardar datos en memoria antes de grabarlos en disco.
- Cuando preferís el equilibrio probabilístico _(azar)_ en lugar de del reequilibrio determinístico _(en cada operación, se corrige y se verifica para que cumpla con ciertas reglas, como en un AVL-Tree o Red-Black-Tree)_.
- Si se requiere una estructura fácil de versionar, sin presenciar reestructuraciones como las rotaciones de un árbol.

### Cuándo NO usarlo

- Con una memoria limitada/pocos registros, debido a que se requieren muchos punteros extras.
- Si la cantidad de datos son aptos para estar en un array, y se realizan pocas inserciones.
- Necesitas garantías acerca del peor caso, porque esta estructura no ofrece límites exactos para esta medida de complejidad.
- Cuando se precisan estructuras de datos compactas en disco.
- En sistemas de tiempo real estrictos, donde no es tolerable que una operación tarde demasiado, al ser probabilística, no puede garantizar eso.

### Comparaciones
| Estructura       | Ventaja frente a Skip List                                      | Desventaja frente a Skip List                       |
| ---------------- | ----------------------------------------------------------- | ----------------------------------------------- |
| Lista enlazada            | Menor uso de memoria _(un puntero por nodo)_               | La búsqueda es O(n), sin niveles para saltar                |
| Array   | Menor uso de memoria y acceso por índice                          | Inserciones/eliminaciones cuestan O(n)                           |
| Árbol balanceado | Garantiza O(log n) en el peor caso, no solo en promedio | Más complejo de implementar y de utilizarlo de forma concurrente       |
| Tabla de hash          | Acceso promedio O(1), más rápido para búsqueda puntual         | No mantiene orden ni permite recorridos por rango                   |

### Ventajas / desventajas

Las ventajas son:
- Implementación simple, sin rotaciones ni lógica de rebalanceo, y es fácil constituir la concurrencia de forma segura.
- Mantiene los elementos ordenados y permite recorridos por rango eficientes.
- Se adapta bien a las inserciones y eliminaciones sin necesidad de reequilibrarse.
- Complejidad temporal esperada es O(log n) para búsqueda, inserción y eliminación.

Por otro lado, las desventajas son las siguientes:
- No es determinista, porque el peor caso de las operaciones podría degradarse a O(n) _(todos los nodos quedan en un mismo nivel)_.
- Mayor uso de memoria por nodo _(el doble que una lista enlazada simple)_.
- No es apta para sistemas de tiempo real estrictos.
- Son unidireccionales, no pueden recorrer hacia atrás.

### Señales de reconocimiento
- "Necesito una alternativa más simple que un árbol balanceado"
- "Hay múltiples hilos/procesos leyendo y escribiendo al mismo tiempo"
- "El orden importa, pero no se necesita garantía absoluta de peor caso"
- "Queremos un conjunto ordenado por puntaje/ranking"
- "Se necesita consultar todos los elementos de un rango"

## 5. Relaciones y extensiones

### Variantes
- Indexada
- Concurrente
- Determinística

### Relación con otras estructuras
- El nivel 0 funciona como una Linked List, contiene todos los elementos y funciona como una linked list  convencional.
- Es similar al AVL-Tree o Red-Black Tree, ya que permiten realizar operaciones en O(log n).
- En su caso de uso de índice en memoria, se asimila al rol que cumple B-Tree en bases de datos relacionales.

### Notas avanzadas
- **Concurrencia:** Ideal para sistemas multiprocesador. Permite múltiples lecturas y escrituras simultáneas con muy baja contención de bloqueos, siendo mucho más simple de adaptar que los árboles balanceados.
- **Persistencia:** Su lógica probabilística facilita la creación de versiones "aplicativas" (persistentes) de la estructura, permitiendo conservar estados anteriores de los datos tras una modificación sin necesidad de reescribir toda la lista.

## 6. Referencias y recursos

- [Skip Lists: A Probabilistic Alternative to Balanced Trees](https://15721.courses.cs.cmu.edu/spring2018/papers/08-oltpindexes1/pugh-skiplists-cacm1990.pdf)


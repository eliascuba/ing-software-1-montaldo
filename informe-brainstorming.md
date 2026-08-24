<div align="center">

<img src="Logo-fiuba.png" width="250" alt="Logo FIUBA">

<br><br>

# UNIVERSIDAD DE BUENOS AIRES
## FACULTAD DE INGENIERÍA

<br><br>

### Ingeniería de Software 1
### Cátedra Montaldo

<br><br>

# Informe Brainstorming

<br><br>

### Alumno

| Elías Josué Cuba Díaz | 106933 |

</div>

---

Cada grupo elige una app conocida, por ejemplo:

cabify (didi/uber)

¿Quiénes son sus usuarios?

¿Qué problema resuelve la app?

¿Qué funcionalidades principales tiene?

¿Qué cosas no funcionan tan bien o podrían mejorar?

Objetivo: Encontrar al menos 10 problemas o oportunidades. (gemini)

Brainstorming de mejoras
Reglas del brainstorming:
no criticar ideas
cantidad > calidad
construir sobre ideas de otros
aceptar ideas locas
Objetivo: 20 ideas nuevas

Elegir la mejor idea
Elegir una idea final
Responder:
¿Qué problema resuelve?
¿Para quién?
¿Por qué sería valiosa?

Entregable
Individualmente terminar el trabajo y presentar en campus un pdf con:
App elegida
Problemas detectados
Lista de ideas
Idea Final
Sobre la idea final se deberá hacer una encuesta o entrevista de usuarios

Diseñar preguntas de entrevista
No preguntar sobre la idea directamente
Preguntar sobre comportamientos reales.
Entrevistar usuarios

Conclusión
¿El problema existe?
¿Nuestra idea ayuda a resolverlo?
¿Deberíamos cambiar algo?



## 1. Demostraciones Teóricas

El problema del Hitting-Set (versión de decisión) se define de la siguiente manera: Dado un conjunto $A$ de $n$ elementos, $m$ subconjuntos $B_1, B_2, \dots, B_m \subseteq A$, y un número entero $k$, queremos saber si existe un subconjunto $C \subseteq A$ tal que $|C| \le k$ y $C \cap B_i \neq \emptyset$ para todo $1 \le i \le m$.

### 1.1. Demostración de Pertenencia a la clase NP

Para demostrar que un problema de decisión pertenece a la clase NP, debemos demostrar que dada una posible solución (o certificado), podemos verificar si es correcta en **tiempo polinomial** mediante una máquina de Turing determinística.

**Certificado:** Un subconjunto $C \subseteq A$.

**Algoritmo Verificador:**
1. Verificamos que la cantidad de elementos en el conjunto $C$ sea menor o igual a $k$ ($|C| \le k$). Esto toma un tiempo $\mathcal{O}(|C|) \le \mathcal{O}(n)$.
2. Para cada subconjunto $B_i$ (donde $1 \le i \le m$), verificamos que la intersección $C \cap B_i$ no sea vacía. Y para eso, propusimos las siguientes hipótesis: 
   - Como mucho, hay $m$ subconjuntos.
   - Cada subconjunto $B_i$ tiene a lo sumo $n$ elementos.
   - El tamaño de $C$ es a lo sumo $n$.
   - Verificar si hay al menos un elemento de $B_i$ en $C$ toma en el peor de los casos $\mathcal{O}(n)$. Por lo tanto, validar todos los subconjuntos toma un tiempo de $\mathcal{O}(m \cdot n)$.

**En resumen**, el tiempo total de verificación está acotado por $\mathcal{O}(n + m \cdot n)$, lo cual es claramente un tiempo polinomial respecto al tamaño de la entrada. 
Por lo tanto, **el problema del Hitting-Set pertenece a la clase NP**.


### 1.2. Demostración de NP-Completitud

Para demostrar que el Hitting-Set es NP-Completo, debemos demostrar que:
1. Pertenence a NP (ya demostrado en el punto 1.1).
2. Es NP-Hard, lo cual demostraremos realizando una reducción polinomial desde un problema que ya sabemos que es NP-Completo hacia el Hitting-Set ($X \le_P \text{Hitting-Set}$).

El problema NP-Completo que elegimos para realizar la reducción es el **Vertex Cover (Cubrimiento de Vértices)**. 
* *Definición de Vertex Cover:* Dado un grafo $G = (V, E)$ y un entero $k$, ¿existe un subconjunto de vértices $V' \subseteq V$ tal que $|V'| \le k$ y para toda arista $(u, v) \in E$, al menos uno de los vértices $u$ o $v$ pertenece a $V'$?

#### Proceso de Reducción Polinomial ($VC \le_P HS$)

Dada una instancia arbitraria de Vertex Cover definida por un grafo $G = (V, E)$ y un entero $k$, construimos una instancia equivalente de Hitting-Set de la siguiente forma:

* El universo de elementos $A$ será el conjunto de vértices del grafo: $A = V$.
* Los subconjuntos $B_1, B_2, \dots, B_m$ serán las aristas del grafo. Es decir, por cada arista $e = (u,v) \in E$, creamos un subconjunto $B_e = \{u, v\}$.
* El valor de la cota $k$ se mantiene exactamente igual.

**Complejidad de la reducción:**
Al crear el conjunto $A$ toma un tiempo proporcional a la cantidad de vértices $\mathcal{O}(|V|)$. Al crear los subconjuntos $B_e$ toma un tiempo proporcional a la cantidad de aristas $\mathcal{O}(|E|)$. 
Entonces, la construcción completa se realiza en **tiempo polinomial** $\mathcal{O}(|V| + |E|)$.

#### Equivalencia de las instancias

Para completar la demostración, debemos probar que el grafo $G$ tiene un Vertex Cover de tamaño *menor o igual* a $k$ **si y solo si** la instancia construida tiene un Hitting-Set de tamaño *menor o igual* a $k$.

#### ($\implies$) Vertex Cover implica Hitting-Set

Supongamos que $V' \subseteq V$ es un Vertex Cover de $G$ con $|V'| \le k$. Por definición, para toda arista $e = (u,v) \in E$, se cumple que $u \in V'$ o $v \in V'$ (o ambos). 
En nuestra instancia de Hitting-Set, tomamos como solución el conjunto $C = V'$. Sabemos que $|C| \le k$. Para cada subconjunto $B_e = \{u,v\}$, como al menos $u$ o $v$ pertenecen a $V'$, entonces $B_e \cap C \neq \emptyset$. 
**Por lo tanto**, $C$ es un Hitting-Set válido.

#### ($\impliedby$) Hitting-Set implica Vertex Cover

Supongamos que existe un Hitting-Set $C \subseteq A$ con $|C| \le k$. Por definición, $C$ intersecta a todos los subconjuntos $B_e$.
Si tomamos en el grafo el conjunto de vértices $V' = C$, sabemos que $|V'| \le k$. Además, como para todo subconjunto $B_e = \{u,v\}$ (que representa una arista) se cumple que $C \cap \{u,v\} \neq \emptyset$, significa que al menos el vértice $u$ o el vértice $v$ pertenecen a $C$ ($V'$). Por lo tanto, $V'$ cubre todas las aristas de $E$, convirtiéndose en un Vertex Cover válido.

**Conclusión:** Como Hitting-Set está en NP y un problema conocido NP-Completo (Vertex Cover) se puede reducir a él polinomialmente.
Y por lo tanto queda demostrado que **el Hitting-Set Problem es NP-Completo**.

---
## 2. Resolución del problema con Backtracking

En cada llamada recursiva, se busca un conjunto que todavía no está cubierto, y se ramifica probando agregar cada uno de sus jugadores al equipo actual.


#### Poda

```
if len(equipo_actual) >= len(mejor_equipo):
   return mejor_equipo
```

Si el equipo actual ya tiene tantos o más jugadores que la mejor solución conocida, esta rama nunca va a mejorar, por lo tanto se omite.


#### Caso base

```
subconjunto_no_cubierto = None
for subconjunto in subconjuntos:
   if not subconjunto.intersection(equipo_actual):
      subconjunto_no_cubierto = subconjunto
      break

if subconjunto_no_cubierto is None:
   return equipo_actual.copy()
```

Si todos los conjuntos están cubiertos, el equipo actual es una solución válida. Por la poda se sabe que es mejor que la anterior.



#### Ramificación

```
for jugador in subconjunto_no_cubierto:
   equipo_actual.add(jugador)
   mejor_equipo = backtracking_hitting_set(subconjuntos, equipo_actual, mejor_equipo)
   equipo_actual.remove(jugador)

return mejor_equipo
```


Para el primer conjunto no cubierto y se prueba agregando cada uno de sus jugadores. La clave es que ese conjunto tiene que ser cubierto por alguien, entonces no se pierden soluciones al restringir a los jugadores de ese conjunto(cualquier hitting set válido tiene que incluir al menos uno de ellos). Después de explorar la rama con un jugador, se remueve y prueba el siguiente. Así se analizan todas las combinaciones posibles dentro de la poda.

---

## 3. Resolucion del problema con Programación Lineal

Un conjunto de jugadores convocados $A={1,2,…,n}$, un conjunto de medios/periodistas (o grupos de presión) ${B_1, B_2, …, B_m}$ donde cada $B_i ⊆ A$ contiene los jugadores que el medio $i$ quiere ver jugar.
Queremos elegir el conjunto más chico de jugadores $C \subseteq A$ tal que:
$C \cap B_i \neq \emptyset$

### 3.1 Variables de Decisión

Definimos una variable binaria por jugador:

$x_j$ para cada jugador $j$ del conjunto de convocados $A$

$x_j = 1$ si el jugador $j$ juega el partido.  
$x_j = 0$ si el jugador $j$ no juega el partido.

Con $x_j ∈ \{0,1\}$ para todo $j ∈ A$

### 3.2 Funcion Objetivo

Necesitamos menor conjunto posible para no desperdiciar el amistoso.  
Entonces minimizamos la cantidad de jugadores usados para “calmar” a la prensa:

$$min \sum_{j∈A} x_j$$

### 3.3 Restricciones

Para cada medio $i$, al menos uno de los jugadores que ese medio quiere debe estar elegido.  
Si $B_i$ es el subconjunto pedido por el medio i:
$$
\sum_{j \in B_i} x_j \ge 1
\qquad \forall i = 1, \dots, m
$$

### 3.4 Modelo completo

Entonces el modelo completo queda:

Variables binarias:
$$x_j ∈ \{0,1\} ∀ j ∈ A$$

Funcion objetivo:
$$min \sum_{j∈A} x_j$$
Restricciones:
$$
\sum_{j \in B_i} x_j \ge 1
\qquad \forall i = 1, \dots, m
$$

---

## 4. Resolución del problema con Greedy

Tomamos como criterio que el óptimo local sería el jugador más solicitado.

#### Elección del óptimo local

```
def jugador_mayor_prioridad(conjuntos):

   if not conjuntos:
      return None

   prioridades = dict()
   for conjunto in conjuntos:
      for jugador in conjunto:
         prioridades[jugador] = prioridades.get(jugador, 0) + 1
   return max(prioridades, key=prioridades.get)
```

Dado un listado de listas que son las elecciones de cada periodista, se elige el jugador con más solicitudes.

#### Actualización del jugador más requerido

```
def hitting_set_greedy(conjuntos):
    
   resultado = []
   jugador_prioritario = jugador_mayor_prioridad(conjuntos)
   
   while jugador_prioritario:
      conjuntos = [c for c in conjuntos if jugador_prioritario not in c]
      resultado.append(jugador_prioritario)
      jugador_prioritario = jugador_mayor_prioridad(conjuntos)

   return resultado
```
Dado un jugador con prioridad máxima, se omiten los conjuntos donde se encuentre dicho jugador. Éste jugador formará parte del mejor equipo.
Posteriormente se recalculará el jugador prioritario con los conjuntos restantes.

Esto no garantiza la solución óptima porque cuando hay 2 jugadores con la misma prioridad máxmia, el algoritmo elige al primero que encuentra.
Por ejemplo, 5 jugadores (A,B,C,D,E) y 5 periodistas donde sus elecciones son:

E,B
A,B
A,B
A,C
D,C

Se puede ver que las mejores opciones son A y B. La desventaja en elegir A es que luego sí o sí debe elegir otros 2 jugadores diferentes. Caso contrario con elegir B que da como resultado 2 jugadores mínimos en lugar de 3.

#### Complejidad

Su complejidad es $O(j \cdot p)$, donde $j$ es la cantidad de jugadores que eligen los periodistas y $p$ es la cantidad de periodistas.
Esto es porque para encontrar al jugador con prioridad máxima se itera por cada periodista y a lo sumo un periodista puede tener todos los jugadores posibles en su lista.

---

## 5. Toma de tiempos

#### Tiempos para Backtracking

![](imagenes/backtracking.png)

#### Tiempos para Programación Lineal

![](imagenes/lineal/lineal%205.png)
![](imagenes/lineal/lineal%207.png)
![](imagenes/lineal/lineal%2010%20pocos.png)
![](imagenes/lineal/lineal%2010%20varios.png)
![](imagenes/lineal/lineal%2010%20todos.png)
![](imagenes/lineal/lineal%2015.png)
![](imagenes/lineal/lineal%2020.png)
![](imagenes/lineal/lineal%2050.png)
![](imagenes/lineal/lineal%2075.png)
![](imagenes/lineal/lineal%20100.png)
![](imagenes/lineal/lineal%20200.png)

#### Tiempos para Greedy
![](imagenes/greedy.png)


#### Conclusiones

##### Programación Lineal (6ms → 1400ms)
El crecimiento viene del tamaño del modelo de optimización: más jugadores significa más variables y restricciones en el LP. El solver resuelve un sistema matricial cuya complejidad crece con el tamaño del problema. Aun así es relativamente estable.

##### Backtracking (0.02ms → 56.000ms)
El tiempo explota porque el espacio de búsqueda es exponencial. Por otra parte, se ve algo particular en los casos con 10 periodistas.

- 10_pocos: solución óptima = 3. El árbol se poda rápido, pocas ramas válidas.
- 10_varios: solución óptima = 6. Hay más combinaciones de tamaño 6 que explorar antes de confirmar que ninguna de tamaño 5 funciona.
- 10_todos: solución óptima = 10. El peor caso posible. Se tienen que descartar todas las combinaciones de tamaño 1, 2, 3, ..., 9 antes de llegar a la respuesta.

##### Greedy (0.08ms → 0.8ms)
Es el más rápido. En cada iteración toma una única decisión localmente óptima (el jugador que "cubre" más relaciones pendientes) y nunca retrocede. La contrapartida es que el greedy no garantiza el óptimo global.

---

# Anexo: Reentrega

## 1. Validador polinomial en código (Punto 1)

A continuación, se presenta la implementación en Python del algoritmo certificador explicado en la sección 1.1. Este validador recibe una instancia del problema (el universo de jugadores, los pedidos de la prensa y el límite $k$) junto con un **certificado** (el conjunto $C$ propuesto como solución) y verifica en tiempo polinomial si es válido.

```python
def validador_hitting_set(universo, subconjuntos, k, certificado):
    """
    Verifica si un 'certificado' es una solución válida para el Hitting-Set.
    - universo: set con todos los jugadores disponibles (A).
    - subconjuntos: lista de sets con los pedidos de los medios (B_i).
    - k: tamaño máximo permitido para la solución.
    - certificado: set con los jugadores propuestos para el equipo (C).
    """
    
    # 1. Verifica que el tamaño del conjunto C sea menor o igual a k
    if len(certificado) > k:
        return False
        
    # Valida que los jugadores propuestos existan en el universo A
    if not certificado.issubset(universo):
        return False

    # 2. Verifica que C tenga al menos un elemento de cada B_i
    for subconjunto in subconjuntos:
        # Si la intersección entre el certificado y el pedido del medio es vacía
        if not certificado.intersection(subconjunto):
            return False # El certificado falla en cubrir a este medio
            
    # Si pasa todas las validaciones, el certificado es correcto
    return True
```

### Explicación del código y su complejidad

Tal como se detalló en el planteo original:

1. La operación ```len(certificado) > k``` se ejecuta en $\mathcal{O}(1)$ en Python, pero teóricamente verificar el tamaño de $C$ toma tiempo $\mathcal{O}(|C|) \le \mathcal{O}(n)$.
2. La validación ```certificado.issubset(universo)``` asegura la correctitud de los datos en tiempo $\mathcal{O}(|C|)$.
3. El ciclo ```for``` itera sobre los $m$ subconjuntos. En cada iteración, la operación ```certificado.intersection(subconjunto)``` compara los elementos de ambos sets, tomando en el peor de los casos un tiempo proporcional al tamaño del subconjunto $\mathcal{O}(n)$.

Dado que el bucle se ejecuta $m$ veces y su interior cuesta a lo sumo $\mathcal{O}(n)$, la complejidad total del bloque de validación está acotada por $\mathcal{O}(m \cdot n)$. Por ende, el validador completo ejecuta en tiempo polinomial $\mathcal{O}(n + m \cdot n)$, confirmando que el problema pertenece a la clase NP.

## 2. Generación de sets de datos y mediciones empíricas (Punto 3)

Para evaluar el comportamiento y la complejidad real de nuestro algoritmo de **Backtracking**, desarrollamos un script en Python (`generador.py`) que crea instancias del problema de forma aleatoria. 

**Estrategia de generación:**
El generador toma como universo a los $n = 43$ jugadores disponibles en el plantel. Para generar una instancia de tamaño $m$ (donde $m$ es la cantidad de periodistas/medios), el script genera $m$ subconjuntos. Cada subconjunto se forma eligiendo aleatoriamente entre 2 y 7 jugadores del universo. 

Se generaron sets de datos propios con $m \in \{5, 10, 15, 20, 25, 30\}$ para observar cómo escala el tiempo de ejecución a medida que aumenta la cantidad de restricciones.

### Mediciones y Resultados

Se ejecutó el algoritmo de Backtracking (`tp3.py`) utilizando estos nuevos sets de datos. Los resultados obtenidos fueron los siguientes:

| Archivo | Cantidad de Periodistas ($m$) | Tiempo de ejecución (ms) |
| :--- | :---: | :---: |
| `test_propio_5.txt` | 5 | [TU TIEMPO ACÁ] |
| `test_propio_10.txt` | 10 | [TU TIEMPO ACÁ] |
| `test_propio_15.txt` | 15 | [TU TIEMPO ACÁ] |
| `test_propio_20.txt` | 20 | [TU TIEMPO ACÁ] |
| `test_propio_25.txt` | 25 | [TU TIEMPO ACÁ] |
| `test_propio_30.txt` | 30 | [TU TIEMPO ACÁ] |

### Análisis de Tiempos

*(Insertar gráfico generado)*
![](imagenes/grafico_reentrega.png)

**Conclusión empírica:**
Como se puede observar en el gráfico y en la tabla, el tiempo de ejecución crece drásticamente a medida que aumenta la cantidad de periodistas ($m$). Esto se condice perfectamente con la complejidad temporal teórica esperada de un algoritmo de Backtracking puro $\mathcal{O}(2^n)$. Si bien nuestras podas ayudan significativamente a reducir el espacio de búsqueda (evitando evaluar ramas que superan el tamaño del óptimo parcial), la naturaleza **NP-Completo** del problema provoca que, para instancias más grandes, la combinatoria vuelva impracticable resolver el problema de manera exacta en tiempos razonables.

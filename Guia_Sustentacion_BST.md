# Guía de Estudio y Sustentación: Árboles Binarios de Búsqueda (BST)

Esta guía está diseñada para prepararte para la sustentación del ejercicio. Aborda los conceptos teóricos clave y explica de forma sencilla las decisiones de diseño tomadas en el código que modificamos.

## 1. Conceptos Básicos: El Árbol Binario de Búsqueda (BST)

> [!NOTE]
> Un BST es una estructura de datos basada en nodos donde cada nodo tiene como máximo dos hijos (izquierdo y derecho).

La regla de oro del BST es:
- El subárbol **izquierdo** de un nodo solo contiene valores **menores** que el valor del nodo.
- El subárbol **derecho** de un nodo solo contiene valores **mayores** que el valor del nodo.

### Manejo de Valores Repetidos (El contador `count`)
En el ejercicio, se pidió permitir la inserción de valores repetidos. En lugar de insertar un nodo extra a la izquierda o a la derecha (lo cual desbalancearía el árbol y complicaría la búsqueda), utilizamos un enfoque más elegante: **un contador (`count`) dentro de cada nodo**.

*   **¿Cómo sustentarlo ante tu profesor?** 
    > *"Decidí agregar un atributo `count` a la clase Node. Cuando se intenta insertar un valor que ya existe en el árbol, el código simplemente detecta la coincidencia e incrementa el contador en lugar de instanciar un nodo nuevo. Esto ahorra memoria RAM y mantiene la estructura y altura del árbol intactas, haciendo que las búsquedas futuras sigan siendo igual de rápidas"*.

---

## 2. Los Recorridos (Traversals)

Los recorridos determinan en qué orden visitamos y mostramos los nodos del árbol. Modificamos cada uno de ellos para que el código imprima la clave (el número) tantas veces como lo indique su variable `count`.

*   **Inorder (Inorden - Izquierda, Raíz, Derecha):** Visita el árbol empezando por los valores más bajos. **Punto clave:** En un BST, un recorrido inorder SIEMPRE te dará los datos ordenados de menor a mayor.
*   **Preorder (Preorden - Raíz, Izquierda, Derecha):** Visita la raíz antes que sus hijos. Es muy útil para crear una copia (clon) exacta del árbol.
*   **Postorder (Postorden - Izquierda, Derecha, Raíz):** Visita a los hijos antes que a la raíz. Es el método ideal si quisieras eliminar todo el árbol desde las hojas hacia arriba sin dejar "nodos huérfanos".
*   **Level Order (Por Niveles o Anchura):** Imprime el nivel 1 (raíz), luego el nivel 2, etc. Nuestro código lo hace calculando primero la altura total ($h$) y luego haciendo un ciclo `for` para imprimir cada nivel individualmente.

---

## 3. Eliminación de un Nodo (El Predecesor Inorder)

> [!IMPORTANT]
> Esta es una pregunta clásica y casi segura de sustentación. Tienes que saber explicar cómo funciona la eliminación, especialmente el caso complejo donde el nodo tiene dos hijos.

Cuando eliminamos un nodo, si tiene múltiples copias (es decir, `count > 1`), nuestro código entra en la condicional y simplemente hace `count -= 1`. Pero si debemos eliminarlo por completo, hay 3 escenarios posibles:
1.  **Es una hoja (0 hijos):** Simplemente se borra retornando `None`.
2.  **Tiene 1 hijo:** El hijo toma el lugar del padre que fue eliminado (el abuelo se conecta directamente con el nieto).
3.  **Tiene 2 hijos (EL CASO COMPLEJO):** No podemos simplemente borrarlo porque dejaríamos a los dos subárboles de abajo desconectados del resto del árbol.

**La Solución Obligatoria: El Predecesor Inorder**
El ejercicio de tu archivo de texto te pidió explícitamente usar el *predecesor inorder* para reemplazar al eliminado.
*   **¿Qué es el Predecesor Inorder?** Es el nodo con el valor **más grande dentro del subárbol izquierdo**. (Lo encontramos yendo a la izquierda una vez desde el nodo que queremos borrar, y luego bajando todo a la derecha hasta no poder más usando la función `max_value_node`).
*   **¿Por qué usamos este nodo en particular?** Porque al ser "el mayor de los menores", podemos subirlo a tomar el lugar de la raíz eliminada y **garantizamos que la regla de oro del BST no se rompa**: el nodo seguirá siendo mayor que todo lo que quedó a su izquierda, pero menor que todo lo que está a su derecha.

---

## 4. Análisis de Complejidad Asintótica (Las famosas Big-O, $\Omega$, $\Theta$)

> [!TIP]
> Los profesores de algoritmia suelen preguntar la diferencia entre estas tres métricas. Explícalas con seguridad para demostrar verdadero dominio matemático.

*   **$T(n)$:** Es la Función de Tiempo. Representa exactamente cuántas operaciones (pasos matemáticos) toma el algoritmo en base a $n$ (la cantidad de nodos).
*   **Big-O ($O$):** Representa el "Peor de los casos" (El límite superior). Responde a la pregunta: *¿Si los datos ingresan en el peor orden posible, qué tan lento será?*
*   **Big-Omega ($\Omega$):** Representa el "Mejor de los casos" (El límite inferior). Responde a la pregunta: *¿Si los datos son ideales, qué tan rápido acabará?*
*   **Big-Theta ($\Theta$):** Representa el "Caso Promedio/Exacto". Solo existe si el peor y el mejor caso crecen exactamente a la misma velocidad.

### Respuestas exactas para sustentar cada método:

1. **Recorridos (Inorder, Preorder, Postorder, Leaf Nodes, Non-Leaf Nodes):**
   * Al imprimir el árbol completo, el código obligatoriamente debe visitar cada uno de los $n$ nodos en el árbol exactamente una vez, independientemente de su forma.
   * **Complejidad:** Al ser dependiente directamente de $n$, su límite es $O(n)$, $\Omega(n)$ y $\Theta(n)$. Es decir, su tiempo de ejecución crece de forma **Lineal**.

2. **Delete Node (Eliminación):**
   * El tiempo que toma borrar no depende de todos los nodos, sino de tener que bajar hasta encontrarlo. Depende de la altura del árbol ($h$).
   * **Peor caso $O(n)$:** Ocurre si insertaron los datos ya ordenados (ej. 10, 20, 30, 40). El árbol nunca se divide en ramas, sino que forma una "línea recta" hacia un lado. Para eliminar el último, recorrerá los $n$ elementos.
   * **Mejor caso $\Omega(1)$ / $\Omega(\log n)$:** Toma 1 solo paso si tienes suerte y te piden eliminar directamente la raíz y es una hoja. Si el árbol estuviera perfectamente balanceado y espeso, encontrar cualquier nodo tomaría logaritmo base 2 de $n$ pasos ($\log n$).
   * **Conclusión:** No hay un $\Theta$ único para la eliminación, ya que oscila drásticamente entre ser sumamente rápido en un árbol balanceado y muy lento en un árbol desbalanceado (lineal).

3. **Print Level Order (Recorrido por Niveles modificado):**
   * Nuestro algoritmo averigua primero la altura del árbol y luego, por cada nivel que existe, comienza a bajar desde la raíz para encontrar e imprimir los nodos correspondientes.
   * **Peor caso $O(n^2)$:** Si el árbol es una línea recta hacia la derecha (desbalanceado extremo), para imprimir el nivel 5 tiene que bajar pasando por 1, 2, 3 y 4. Repite este mismo recorrido desde la raíz por cada nivel. La sumatoria matemática de estos pasos redundantes da una complejidad **Cuadrática** ($O(n^2)$).

---

## 💡 Consejos de Oro para el momento de presentar
1. **Ejecuta en orden:** Recuerda que Jupyter Notebook guarda en memoria. Si el profesor te pide que reinicies y pruebes el código, no vayas directo a darle "Run" al hardcode. Explícale *"Primero debo compilar las definiciones de la clase y funciones en la primera celda"*, la corres, y luego corres la segunda. Si corres la segunda primero dará error.
2. **Dibuja mentalmente (o en un papel) la eliminación:** Si el profesor te pide eliminar el valor "50" del hardcode (que en nuestro ejercicio arranca siendo la primera raíz que ingresa con dos hijos), muestra con las manos cómo el algoritmo entra al hijo izquierdo (30) y luego baja todo a la derecha hasta el final encontrando a su predecesor inorder (40). El 40 sube y reemplaza al 50.

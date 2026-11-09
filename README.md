# Benchmark de algoritmos de ordenamiento

## Metodología

Se probaron listas de 500, 1000, 2000 y 4000 elementos en cuatro órdenes iniciales. Para cada combinación, los cinco algoritmos recibieron copias de la misma lista. Se usó la semilla 42, se midió solo el ordenamiento y se tomó la mediana de tres ejecuciones. Cada resultado se comprobó con `sorted()`.

## Resultados con 4000 elementos

Tiempos en milisegundos. El CSV contiene las mediciones de todos los tamaños en segundos.

| Orden inicial | Selección | Inserción | Burbuja | Quicksort | Heapsort |
|---|---:|---:|---:|---:|---:|
| Aleatorio | 140,253 | 156,602 | 355,294 | **2,288** | 4,044 |
| Ordenado | 131,569 | 0,171 | **0,085** | 1,161 | 3,928 |
| Inverso | 138,923 | 273,983 | 400,597 | **1,186** | 3,702 |
| Casi ordenado | 136,923 | 3,369 | 158,586 | **1,196** | 4,045 |

## 1. ¿Cuál fue más rápido y cuál más lento?

| Orden inicial | Más rápido | Más lento | Explicación |
|---|---|---|---|
| Aleatorio e inverso | Quicksort | Burbuja | Quicksort divide la lista en particiones. Burbuja hace muchos intercambios entre vecinos. |
| Ordenado | Burbuja | Selección | Burbuja con bandera termina tras una pasada sin intercambios. Selección sigue buscando el mínimo en cada tramo. |
| Casi ordenado, 500 y 1000 | Inserción | Selección | Inserción aprovecha el orden previo. Selección mantiene su trabajo cuadrático. |
| Casi ordenado, 2000 y 4000 | Quicksort | Burbuja | Quicksort divide el trabajo. Burbuja puede necesitar muchas pasadas si hay elementos lejos de su posición. |

## 2. ¿Qué pasó al duplicar el tamaño?

Cada valor indica cuántas veces aumentó el tiempo al pasar de **2000 a 4000** elementos.

| Orden inicial | Selección | Inserción | Burbuja | Quicksort | Heapsort |
|---|---:|---:|---:|---:|---:|
| Aleatorio | 4,03 | 3,98 | 4,15 | 2,18 | 2,22 |
| Ordenado | 3,96 | 2,01 | 2,05 | 2,17 | 2,20 |
| Inverso | 4,03 | 4,21 | 4,20 | 2,07 | 2,34 |
| Casi ordenado | 4,01 | 2,95 | 3,81 | 2,20 | 2,20 |

Selección creció cerca de **4 veces**, como se espera para O(n²). Inserción y Burbuja también se acercaron a 4 con datos aleatorios e inversos. Con datos ordenados crecieron cerca de **2 veces** porque su mejor caso es O(n). Con datos casi ordenados, el trabajo varió según cuántos elementos quedaron fuera de orden.

Quicksort y Heapsort crecieron cerca de **2,2 veces**, en línea con O(n log n). En los otros saltos del CSV se observa la misma tendencia general, con variaciones entre mediciones.

## 3. ¿Por qué Quicksort suele superar a Heapsort?

Quicksort recorre tramos contiguos de la lista al dividirla, lo que suele favorecer el acceso a memoria. Heapsort tiene que reajustar el montículo después de cada extracción y acceder a posiciones más separadas. Ambos tienen costo promedio O(n log n), pero el trabajo por paso puede ser distinto.

Heapsort garantiza O(n log n) en el peor caso. Quicksort puede llegar a O(n²) si las particiones quedan muy desequilibradas.

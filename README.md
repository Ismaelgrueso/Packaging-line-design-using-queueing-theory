# Diseño de líneas de packing con teoría de colas

Este repositorio incluye un cuaderno de Jupyter donde se compara el rendimiento de dos diseños de una zona de packing utilizando teoría de colas.  
El objetivo es ayudar a decidir si es más conveniente trabajar con **una cola única que alimenta varias estaciones** o con **varias colas dedicadas**, en función de la variabilidad del proceso.

---

## 1. ¿Qué hace el cuaderno?

De forma resumida, el cuaderno:

1. Plantea un escenario de packing (llegadas de pedidos y capacidad de proceso de las estaciones).
2. Calcula el **tiempo medio de espera en cola** para dos diseños:
   - **Solución 1**: una cola única que alimenta varias estaciones en paralelo.
   - **Solución 2**: varias líneas con colas dedicadas, cada una con su propia estación.
3. Repite el cálculo para distintos niveles de **variabilidad del proceso** (CVP).
4. Devuelve:
   - Una tabla con los resultados numéricos.
   - Una gráfica que compara el tiempo de cola de cada solución.

La idea es que puedas ver cómo se comporta cada diseño cuando el proceso de packing es más o menos variable.

---

## 2. Cómo usar el cuaderno

1. Abre el cuaderno `.ipynb` en Jupyter Notebook o JupyterLab.
2. Ejecuta todas las celdas en orden.
3. Al final del cuaderno encontrarás:
   - Una tabla con los valores calculados para cada nivel de variabilidad.
   - Una gráfica donde se comparan las dos soluciones.

Con la configuración por defecto ya puedes ver un **caso base** sin tocar nada.

---

## 3. Dónde debes poner atención si quieres cambiar el escenario

Si quieres adaptar el análisis a tu propio contexto, fíjate especialmente en:

- La celda donde se definen los **parámetros del modelo**, por ejemplo:
  - Tasa de llegadas de pedidos (`ra`).
  - Capacidad de proceso por estación (`rp`).
  - Número de estaciones en paralelo de la solución de cola única (`m`).
  - Número de líneas dedicadas y reparto de carga entre ellas (`k` y `reparto`).
  - Nivel de variabilidad de las llegadas (`CVA`) y del proceso (`CVP` o rango de CVP).

- El rango de valores de **CVP** que se recorre para construir la tabla y la gráfica.  
  Ajustando este rango puedes estudiar procesos con menos o más variabilidad.

> Importante: si al modificar parámetros aparecen tiempos de cola muy grandes o “infinitos”, significa que la utilización del sistema es demasiado alta (la línea está sobrecargada y la cola tiende a crecer sin límite).

---

## 4. Cómo interpretar los resultados

- La tabla de resultados te muestra, para cada nivel de variabilidad (CVP):
  - El tiempo medio de espera en cola de la **Solución 1 (cola única)**.
  - El tiempo medio de espera de la **Solución 2 (colas dedicadas)**.
  - La diferencia entre ambas.

- La gráfica te permite ver de un vistazo:
  - En qué zona de variabilidad la cola única con varias estaciones se comporta mejor.
  - En qué medida las colas dedicadas penalizan el tiempo de espera cuando el proceso es más variable.

En resumen, este cuaderno es una **herramienta de exploración**: te permite jugar con los parámetros de tu proceso de packing y entender qué diseño de línea es más robusto cuando la variabilidad aumenta.

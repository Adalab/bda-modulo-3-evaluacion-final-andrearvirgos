# **EVALUACIÓN FINAL - MÓDULO 3 (Data Analytics):**

Este repositorio contiene la solución al ejercicio de evaluación final del Módulo 03. El objetivo principal del presente ejercicio es realizar un análisis exploratorio de los datos, su correspondiente limpieza y las visualizaciones para responder a las distintas preguntas planteadas en el enunciado. 

---

## 📂 **Archivos base incluidos en el repositorio:**

- El documento con el enunciado del ejercicio de evaluación.
- Un archivo [Customer Flight Activity.csv] (data/Customer%20Flight%20Activity.csv) que contiene los datos sobre la actividad de vuelo de los clientes de la aerolínea:
    * Número de vuelos reservados.
    * Distancia volada.
    * Acumulación de puntos.
    * Costos.

- Un archivo [Customer Loyalty History.csv] (data/Customer%20Loyalty%20History.csv) que incluye información demográfica de la clientela y sobre el programa de fidelidad de la aerolínea.
    * País, provincia, ciudad.
    * Género, nivel educativo, salario.
    * Tipo de tarjeta usada.
    * Fecha de inscripción y cancelación en el programa.

---

## 🎯 **Objetivo del ejercicio:**
El objetivo es analizar el comportamiento de los clientes dentro del programa de lealtad, combinando ambos datasets y respondiendo preguntas mediante visualizaciones claras y bien justificadas.

---
### El trabajo se divide en varias FASES:

## FASE 1: Exploración y limpieza de los datos:
- Carga y exploración inicial de ambos datasets.  
- Búsqueda y tratamiento de valores nulos.  
- Verificación de consistencia, tipos de datos y detección de outliers.  
- Unión de los dos archivos utilizando el campo **Loyalty Number**.  

## FASE 2: Visualización:
1. ¿Cómo se distribuyen los vuelos reservados por mes a lo largo del año?  
2. ¿Existe relación entre la distancia volada y los puntos acumulados?  
3. ¿Cuál es la distribución de clientes por provincia/estado?  
4. Comparación de salarios promedio según nivel educativo.  
5. Proporción de tipos de tarjeta de fidelidad.  
6. Distribución de clientes por estado civil y género.

![Distribución de vuelos por mes](images/vuelos_por_mes.png) ejemplo de cómo insertar 
---

## 🛠️ **Herramientas utilizadas:**
- *Python*
- *Pandas*
- *Matplotlib / Seabron*
- *Jupyter Notebook*
- *Git & GitHub*

--- 

## 📂 **Carpeta entregables:**
En el repositorio se incluirán:
- Script con el desarrollo del proceso (EDA, limpieza y visualización).
- Gráficas generadas.
- Este archivo *README.md* con la explicación del proyecto.

---

## **Autoría:**
Ejercicio de evaluación del *Módulo 3 del Bootcamp de Data Analytics* de Andrea Rodríguez Virgós.




# Escalado de datos

---

## Descripción

El escalado de datos (o data scaling) es un proceso de transformación que se aplica a los datos numéricos para que sus valores se encuentren en un rango comparable o más adecuado para ciertos algoritmos. En otras palabras, es como ajustar el volumen de una canción antes de escucharla: si está muy baja, no la disfrutarás; si está muy alta, molesta. El escalado busca ese "volumen óptimo" para que los algoritmos entiendan mejor los datos y tomen mejores decisiones.

**¿Por qué es importante?**

Imagina que estás usando un algoritmo para predecir si una persona recibirá un crédito. Tienes variables como:

* Edad (rango de 18 a 70),
* Ingresos (de 1,000 a 100,000),
* Número de hijos (de 0 a 5).

Si usas estos datos sin escalarlos, el algoritmo podría darle demasiada importancia a los ingresos solo porque tiene números más grandes, aunque no sea la variable más relevante. Esto puede sesgar los resultados.

El escalado de datos nivela el terreno para que todos los atributos tengan un peso proporcional y justo.

**¿Para qué tipos de algoritmos es fundamental?**

No todos los modelos se ven afectados por las diferencias de escala, pero hay varios donde el escalado es crucial:

* Modelos basados en distancia, como K-Nearest Neighbors, K-Means o SVM.
* Modelos que utilizan gradientes o regularización, como Regresión Logística o Redes Neuronales.
* PCA y otros métodos de reducción de dimensionalidad, que dependen de la varianza de los datos.

En cambio, modelos como los árboles de decisión (por ejemplo, Random Forest) no necesitan escalado porque son invariantes a la escala.

**Tipos comunes de escalado**

Aquí viene una distinción importante que muchas veces se confunde:

* Normalización (Min-Max Scaling)
Transforma los valores para que estén dentro de un rango específico, normalmente de 0 a 1. Es como ajustar las notas de los estudiantes para que todos queden entre 0 y 20.

* Estandarización (Z-score Scaling)
Transforma los datos para que tengan una media de 0 y una desviación estándar de 1. Esto centra los datos y los hace comparables en términos de "desviación típica". Es útil cuando las variables tienen distribuciones normales o cuando usamos modelos sensibles a la varianza.

Otras técnicas que existen:

MaxAbsScaler: escalar en [-1, 1] sin mover el centro.
RobustScaler: resistente a valores atípicos, usando mediana y rango intercuartílico.
QuantileTransformer: transforma la distribución a uniforme o normal.
PowerTransformer: aplica transformaciones como Box-Cox o Yeo-Johnson para acercar a una distribución normal.

Te mencionaré muchas más tecnicas más adelante y entraremos en detalle una a una, no te preocupes.

**¿Qué pasa si no escalas?**

El modelo puede aprender mal o no aprender nada.
Puede tardar más en converger (entrenarse).
Las métricas pueden ser engañosas.
Algunas variables dominarán injustamente sobre otras.

Escalar los datos es como afinar los instrumentos antes de tocar una sinfonía. No importa cuán buenos sean los músicos (algoritmos), si los instrumentos (datos) están desafinados, el resultado no será armonioso. Por eso, antes de correr un modelo, detente y pregúntate: *"¿Mis datos están bien escalados?"*

---

## Estandarización Z-score

### Descripción:

El escalado Z-score, también conocido como estandarización, es una técnica de transformación de datos que centra la media de cada variable en 0 y ajusta su escala para que la desviación estándar sea 1. Es decir, convierte cada valor en una medida de cuántas desviaciones estándar se encuentra respecto a la media de su variable.

Este método no modifica la forma de la distribución original, pero hace que los valores sean comparables entre variables que originalmente estaban en escalas distintas.

Por ejemplo, si una variable tiene un promedio de 50 y una desviación estándar de 10, un valor de 70 se encuentra dos desviaciones estándar por encima de la media, es decir, su Z-score es 2.

### Formulación:

$$z_i = \frac{x_i - \mu}{\sigma}$$

* Donde:

    * $x_i$: es el valor original de la variable.
    * $\mu$: es la media de la variable.
    * $\sigma$: es la desviación estándar de la variable.
    * $z_i$: es el valor escalado (Z-score).

### Beneficios:

* Comparabilidad: hace que todas las variables tengan la misma escala, centrada en 0, lo que permite compararlas directamente.
* Estabilidad numérica: mejora el rendimiento de modelos que utilizan derivadas o cálculos matriciales.
* Compatible con modelos sensibles a la varianza, como PCA, regresión logística, SVM o redes neuronales.
* Mantiene la forma de la distribución original (aunque no la escala ni la ubicación).

### Caso de Uso:

* Se trabaja con algoritmos basados en gradiente (como regresión lineal, regresión logística, redes neuronales).
* Se aplican técnicas de reducción de dimensionalidad (como PCA).
* Se desea centrar y escalar los datos para mejorar la eficiencia y precisión del modelo.

### Supuestos:

* No requiere una distribución normal, pero es más efectivo cuando la variable sigue (o se aproxima a) una distribución normal.
* No es robusto frente a outliers, ya que la media y la desviación estándar son sensibles a valores extremos. Para casos con valores atípicos, puede preferirse el RobustScaler.

---

## Min-Max (Rango)

### Descripción:

El escalado Min-Max, también conocido como normalización, es una técnica que transforma los valores de una variable para que se ubiquen dentro de un rango específico, comúnmente entre 0 y 1. Es como tomar todos los valores de una variable y “estirarlos” o “comprimirlos” proporcionalmente para que encajen en ese intervalo.

Este tipo de escalamiento es muy útil cuando los datos tienen distribuciones no gaussianas o cuando se requiere que todos los valores estén en un rango fijo. Es especialmente importante en algoritmos que dependen de medidas de distancia o pesos absolutos.

### Formulación:

$$x'_i = \frac{x_i - x_{\text{min}}}{x_{\text{max}} - x_{\text{min}}}$$

* Donde:

    * $x'_i$: es el valor escalado.
    * $x_i$: es el valor original.
    * $x_{\text{min}}$: es el valor mínimo de la variable.
    * $x_{\text{max}}$: es el valor máximo de la variable.

### Beneficios:

* Ajusta todos los valores a un rango específico (usualmente [0, 1]), lo cual es requerido por ciertos modelos o visualizaciones.
* Preserva la forma original de la distribución de los datos.
* Muy útil en modelos basados en distancia, como K-Means o KNN, donde la escala afecta directamente los cálculos.
* Aumenta la eficiencia computacional al reducir el rango de valores manejados.

### Caso de Uso:

* Los datos deben estar en un rango fijo, como [0, 1], por requerimientos de algoritmos o funciones de activación (como sigmoid en redes neuronales).
* Se trabaja con algoritmos basados en distancias como KNN, K-Means, SVM, etc.
* Se van a usar algoritmos sensibles a la magnitud absoluta de los valores.

### Supuestos:

* Supone que conoces el mínimo y máximo reales del conjunto de datos, por lo que puede verse afectado negativamente si aparecen outliers extremos.
* No es robusto a valores atípicos; si hay valores muy grandes o muy pequeños, puede distorsionar la escala del resto de los datos.

---

## Desviación Absoluta Mediana (MAD)

### Descripción:

El escalado por Desviación Absoluta Mediana (conocido como MAD scaling, por Median Absolute Deviation) es una técnica de escalamiento robusta frente a outliers. A diferencia de la estandarización clásica que usa la media y la desviación estándar (ambas sensibles a valores extremos), este método utiliza la mediana como medida de tendencia central y la desviación absoluta mediana como medida de dispersión.

En esencia, convierte cada valor en una medida de cuán lejos está de la mediana, en unidades de desviación absoluta, lo que resulta en una representación más estable y realista cuando hay datos ruidosos o valores atípicos.

### Formulación:

$$x'_i = \frac{x_i - \tilde{x}}{\text{MAD}(x)}$$

* Donde:

    * $x'_i$: es el valor escalado.
    * $x_i$: es el valor original.
    * $\tilde{x}$: es la mediana de la variable.
    * $\text{MAD}(x)$: es la desviación absoluta mediana, definida como:
      $$\text{MAD}(x) = \text{mediana}(|x_i - \tilde{x}|)$$

Es decir, la MAD calcula la mediana de las diferencias absolutas entre cada valor y la mediana original.

### Beneficios:

* Robustez ante outliers: es ideal cuando los datos contienen valores extremos que distorsionarían la media o la desviación estándar.
* No requiere normalidad: funciona bien incluso si la variable tiene una distribución asimétrica o no gaussiana.
* Preserva la estructura de los datos, pero centrándolos alrededor de la mediana.
* Muy útil en contextos donde los valores atípicos no deben influir en el aprendizaje del modelo.

### Caso de Uso:

* Se trabaja con datos ruidosos o con muchos outliers.
* Se desea aplicar algoritmos que son sensibles a la escala, pero no se quiere que los valores extremos dominen el resultado.
* Se está analizando datos financieros, médicos, o experimentales donde los valores atípicos son frecuentes pero no necesariamente erróneos.

### Supuestos:

* No requiere distribución normal.
* No requiere conocer los rangos exactos de los datos.
* Preferido cuando la mediana y la dispersión central son más representativos que la media y la desviación estándar.

---

## Logarítmico

### Descripción:

El escalado logarítmico transforma los datos aplicando la función logaritmo a cada valor. Su objetivo principal es reducir la asimetría y comprimir las diferencias entre valores grandes, haciendo que los datos con crecimiento exponencial o distribución muy sesgada se vuelvan más manejables y cercanos a una distribución normal.

Este tipo de transformación no escala los datos en el sentido tradicional de centrar o acotar a un rango específico, sino que modifica su distribución para hacerla más "suave" y simétrica. Es muy útil cuando se tienen valores positivos con una alta dispersión o colas largas hacia la derecha.

### Formulación:

$$x'_i = \log(x_i)$$

* Donde:

    * $x'_i$: es el valor transformado.
    * $x_i$: es el valor original de la variable.
    * $\log$: representa el logaritmo natural (np.log en Python). También puede usarse base 10 (np.log10) si se desea una escala diferente.

### Beneficios:

* Reduce la asimetría de los datos, haciendo que variables sesgadas positivamente se aproximen más a una distribución normal.
* Comprime grandes rangos de valores, reduciendo la influencia de valores extremadamente grandes.
* Mejora el rendimiento de modelos lineales o algoritmos sensibles a la distribución de los datos (como regresión, clustering, etc.).
* Ayuda a cumplir supuestos de normalidad y homocedasticidad en ciertos modelos estadísticos.

### Caso de Uso:

* Se tienen variables positivas y sesgadas hacia la derecha (por ejemplo, ingresos, precios, tiempo de espera).
* Se observan grandes diferencias entre los valores mínimos y máximos que podrían perjudicar al modelo.
* Se trabaja con modelos que suponen linealidad o normalidad de los datos.
* Se desea estabilizar la varianza o mejorar la simetría de la variable.

### Supuestos:

* Todos los valores deben ser positivos. No se puede aplicar logaritmo a valores negativos o cero. Si hay ceros, una solución común es sumar una constante pequeña:
$x'_i = \log(x_i + 1)$

---

## Robusta con Mediana y Desviación Estándar

### Descripción:

Este método de escalado robusto centra los datos utilizando la mediana y los escala con la desviación estándar. A diferencia de la estandarización clásica (que usa la media), esta variante es más resistente a valores atípicos, ya que la mediana no se ve afectada por extremos. Sin embargo, aún se emplea la desviación estándar, lo que le da un balance entre robustez y sensibilidad a la variabilidad.

Esta técnica es útil cuando se quiere conservar la dispersión natural de los datos, pero evitar que la centralización esté sesgada por outliers.

### Formulación:

$$x'_i = \frac{x_i - \tilde{x}}{s}$$

* Donde:

    * $x'_i$: es el valor escalado.
    * $x_i$: es el valor original.
    * $\tilde{x}$: es la mediana de la variable.
    * $s$: es la desviación estándar de la variable.

### Beneficios:

* Menos sensible a outliers que la estandarización clásica, gracias al uso de la mediana como medida de tendencia central.
* Fácil de implementar y de interpretar.
* Conserva parte de la dispersión original, lo que puede ser útil en modelos que usan la varianza como criterio.
* Ideal para mantener una relación proporcional sin que la media altere el centro debido a extremos.

### Caso de Uso:

* Se desea una solución más robusta que la estandarización tradicional, pero más suave que métodos extremos como el MAD o el rango intercuartílico.
* Se trabaja con datos que tienen outliers moderados.
* El modelo requiere centrado y escalado, pero se quiere evitar que la media distorsione el resultado.

### Supuestos:

* No necesita normalidad ni rangos acotados.
* Asume que la desviación estándar es representativa a pesar de centrarse en la mediana, por lo que si hay outliers severos, puede ser mejor usar métodos más robustos como el MAD o el IQR.

---

## Percentiles (Rango Interpercentílico)

### Descripción:

El escalado por percentiles transforma los datos usando un rango definido entre dos percentiles, en lugar del valor mínimo y máximo absolutos. Una práctica común es usar el percentil 5% y el 95%, lo que reduce la influencia de valores extremos (outliers) y permite mantener la estructura general de los datos.

Este enfoque consiste en recentrar los valores y recalcular su escala relativa dentro de un intervalo más robusto, delimitado por percentiles bajos y altos. Es una excelente alternativa cuando hay valores atípicos, pero aún se desea mantener una relación proporcional similar al escalado Min-Max.

### Formulación:

$$x'_i = \frac{x_i - P_5}{P_{95} - P_5}$$

* Donde:

    * $x'_i$: es el valor escalado.
    * $x_i$: es el valor original.
    * $P_5$: es el percentil 5% de la variable.
    * $P_{95}$: es el percentil 95% de la variable.

*Nota: los valores pueden ajustarse a otros percentiles (por ejemplo, 1% y 99%) dependiendo de cuán conservador se desee ser con los outliers.*

### Beneficios:

* Robusto frente a outliers extremos, mucho más que el escalado Min-Max clásico.
* Preserva la forma general de la distribución de los datos dentro del rango central.
* Proporciona una representación escalada más realista en contextos con datos ruidosos o asimétricos.
* Flexible: puedes ajustar los percentiles usados para adaptarlo a la naturaleza de tus datos.

### Caso de Uso:

* Se tienen datos con valores extremos, pero no se desea eliminarlos.
* Se requiere una transformación similar al Min-Max, pero más robusta y controlada.
* Se trabaja en modelos donde la escala relativa importa, pero se quiere evitar que los outliers afecten el escalado general.

### Supuestos:

* No asume normalidad ni simetría en los datos.
* Asume que los percentiles elegidos (como 5% y 95%) son representativos del comportamiento general de la variable.
* Si se desea usar para predicción en producción, los percentiles deben calcularse en el conjunto de entrenamiento para evitar data leakage.

---

## Robusto por Rangos

### Descripción:

El escalado por rangos transforma los valores de una variable reemplazándolos por su posición relativa (ranking) dentro del conjunto de datos, y luego los escala dividiéndolos por el número total de observaciones. El resultado final son valores que representan la posición relativa de cada observación en una escala de 0 a 1.

Este método es extremadamente robusto a outliers, ya que no se basa en los valores reales, sino en su orden. Es ideal cuando no se quiere asumir ninguna forma de distribución o cuando los valores extremos distorsionan otras métricas como la media o la desviación.

### Formulación:

$$x'_i = \frac{r(x_i)}{n}$$

* Donde:

    * $x'_i$: es el valor transformado.
    * $r(x_i)$: es el rango (posición ordenada) de $x_i$ en el conjunto de datos.
    * $n$: es el número total de observaciones.

### Beneficios:

* Totalmente robusto a outliers, ya que solo se considera el orden de los datos, no sus valores reales.
* Produce una escala estándarizada de 0 a 1, sin necesidad de suponer distribución ni calcular medidas de tendencia o dispersión.
* Es ideal para datos ordinales, ya que respeta el orden sin preocuparse por distancias numéricas.
* Simple de implementar y muy estable para variables no paramétricas.

### Caso de Uso:

* El modelo o análisis es insensible a la magnitud exacta, pero sensible al orden relativo (por ejemplo, algunos algoritmos no paramétricos).
* Se tiene mucho ruido o valores extremos que dificultan otras formas de escalamiento.
* Se desea una normalización simple y robusta sin hacer supuestos sobre la distribución de los datos.

### Supuestos:

* No se requiere normalidad ni simetría.
* Se asume que el orden de los datos es relevante, incluso si las distancias entre ellos no lo son.
* Si hay empates (valores repetidos), el rankdata los maneja asignando promedios de posiciones.

---

## Rango Intercuartílico

### Descripción:

El escalado por rango intercuartílico (IQR) transforma los datos restando el primer cuartil ($Q_1$) y dividiendo por la distancia entre el tercer y el primer cuartil ($Q_3 - Q_1$). Este método se centra en la distribución central de los datos (el 50% intermedio), ignorando por completo los valores extremos.

Es una técnica altamente robusta a outliers, ya que no se ve afectada por valores muy grandes o pequeños. Al transformar los datos en función del rango intercuartílico, se garantiza que la mayor parte de las observaciones estén comparablemente escaladas, sin que los extremos distorsionen el resultado.

### Formulación:

$$x'_i = \frac{x_i - Q_1}{Q_3 - Q_1}$$

* Donde:

    * $x'_i$: es el valor escalado.
    * $x_i$: es el valor original de la variable.
    * $Q_1$: es el primer cuartil (percentil 25%) de la variable.
    * $Q_3$: es el tercer cuartil (percentil 75%) de la variable.

### Beneficios:

* Muy robusto frente a outliers, ya que ignora los extremos al calcular el rango.
* Centra los datos de forma relativa a su distribución interna, en lugar de utilizar valores absolutos como el mínimo o la media.
* Ideal para modelos sensibles a la escala pero no a la forma de la distribución.
* Permite comparar distintas variables incluso si tienen unidades o magnitudes diferentes.

### Caso de Uso:

* Se tienen datos con valores atípicos que podrían distorsionar otros métodos como Min-Max o Z-score.
* Se desea una transformación robusta pero no excesivamente agresiva, conservando la estructura general de la variable.
* Se trabaja con modelos lineales, clustering, PCA u otros algoritmos sensibles a escala, pero con datos ruidosos.

### Supuestos:

* No necesita supuestos de normalidad.
* Asume que la distribución central (entre $Q_1$ y $Q_3$) es representativa del comportamiento de la variable.
* Ideal si se desea mantener el rango central sin aplicar funciones no lineales.

---

## Basado en Información Teórica

### Descripción:

Este tipo de escalado parte del principio de que, en ciertas variables, se conocen los límites mínimo y máximo teóricos que puede alcanzar cada característica. A diferencia de métodos que calculan estos valores desde los datos (como Min-Max), aquí el usuario define manualmente los valores de referencia a partir de conocimiento externo o del dominio.

Esta estrategia transforma los datos reubicando su escala dentro de un rango fijo $[0,1]$, usando los límites inferior ($LI$) y superior ($LS$) definidos previamente. De esta forma, se evita que valores extremos o datos anómalos alteren el proceso de escalado, y se garantiza que el resultado sea consistente con los valores válidos del sistema.

### Formulación:

$$x'_i = \frac{x_i - LI}{LS - LI}$$

* Donde:

    * $x'_i$: es el valor escalado.
    * $x_i$: es el valor original.
    * $LI$: es el límite inferior teórico de la variable.
    * $LS$: es el límite superior teórico de la variable.

### Beneficios:

* Permite aplicar un escalado controlado y estandarizado cuando se conocen los rangos válidos de los datos.
* No se ve afectado por outliers o errores de medición, ya que no depende de los valores observados.
* Mejora la consistencia y trazabilidad en sistemas regulados o con criterios técnicos bien definidos.
* Ideal cuando el objetivo es mantener los datos dentro de un rango lógico preestablecido.

### Caso de Uso:

* Se cuenta con información previa o restricciones normativas sobre los valores que puede tomar cada variable (por ejemplo, edad entre 18 y 35 años).
* Se quiere aplicar un escalado tipo Min-Max, pero sin dejar que valores extremos afecten los límites.
* Se requiere que los datos escalados mantengan una interpretación física o de negocio coherente.

### Supuestos:

* Se debe conocer con certeza el rango válido o permitido para cada variable.
* Se asume que los valores fuera del rango teórico representan errores, ruido o excepciones que no deben condicionar el escalado.

---

## Basada en Función de Densidad

### Descripción:

El escalado basado en función de densidad, también conocido como interpolación por percentiles, transforma los datos según su posición relativa acumulada en la distribución. Utiliza los percentiles clave (por ejemplo, 0%, 25%, 50%, 75%, 100%) y les asigna valores estándar (como 0, 0.25, 0.5, 0.75, 1), luego interpola los valores intermedios.

Esto crea una transformación suave y continua, basada en la forma de la distribución empírica. A diferencia del escalado Min-Max o Z-score, este método no requiere suposiciones sobre la distribución de los datos, y es mucho más robusto a la asimetría o presencia de outliers.

### Formulación:

$$x'_i = \text{interp}(x_i,\ [P_0,\ P_{25},\ P_{50},\ P_{75},\ P_{100}],\ [0,\ 0.25,\ 0.5,\ 0.75,\ 1])$$

* Donde:

    * $x'_i$: es el valor transformado.
    * $x_i$: es el valor original de la variable.
    * $P_k$: representa el percentil $k$% de la variable (por ejemplo, $P_{25}$ es el percentil 25%).
    * $\text{interp}$: es la función de interpolación lineal entre los valores definidos.

### Beneficios:

* Robusto frente a outliers y valores extremos, ya que se enfoca en la distribución acumulada.
* Proporciona un escalado suave y continuo, útil para modelos que prefieren distribuciones uniformes o casi uniformes.
* Ideal para datos no lineales o no gaussianos, donde otros métodos fallan.
* Mejora la representación de los datos para algoritmos sensibles a la escala y la forma (como PCA, clustering, redes neuronales).

### Caso de Uso:

* Se desea transformar datos a un rango estándar (0 a 1) sin usar el mínimo ni el máximo observados directamente.
* La distribución de los datos es asimétrica o multimodal, y se quiere un escalado más natural y progresivo.
* Se busca una alternativa al escalado por cuantiles, pero con control sobre los puntos de interpolación.
* Se tiene una distribución muy ruidosa o con huecos, y se requiere un escalado más suave y continuo que el Rank Scaling.

### Supuestos:

* Se asume que los percentiles definidos representan correctamente la forma de la distribución.
* Requiere definir puntos de anclaje ($P_0$, $P_{25}$, ..., $P_{100}$), pero pueden adaptarse según el dominio o distribución real.
* Puede comportarse mal si los percentiles están muy cercanos (por ejemplo, si $P_{75} = P_{100}$).

---

## Diferencia de medias

### Descripción:

El escalado por diferencia de medias transforma los datos restando la media de cada variable y dividiendo por la diferencia entre su valor máximo y mínimo. Este procedimiento centra los datos alrededor de cero, similar al Z-score, pero en lugar de usar la desviación estándar, utiliza el rango total como medida de dispersión.

Este método resulta útil cuando se desea centrar los datos y al mismo tiempo se quiere evitar la sensibilidad que tiene la desviación estándar frente a los valores atípicos, ya que el rango es más simple de calcular, aunque también puede ser afectado por outliers.

### Formulación:

$$x'_i = \frac{x_i - \bar{x}}{x_{\text{max}} - x_{\text{min}}}$$

* Donde:

* $x'_i$: es el valor transformado.
* $x_i$: es el valor original.
* $\bar{x}$: es la media de la variable.
* $x_{\text{max}}$: es el valor máximo observado.
* $x_{\text{min}}$: es el valor mínimo observado.

### Beneficios:

* Centra los datos alrededor de cero, facilitando el trabajo de modelos que lo requieren (por ejemplo, redes neuronales).
* Utiliza una medida de escala simple (rango), fácil de interpretar.
* Menos sensible a la forma de la distribución que el Z-score.
* Fácil de implementar, sin necesidad de suponer ninguna distribución en los datos.

### Caso de Uso:

* Se busca un escalado centrado en la media, pero más simple o menos técnico que Z-score.
* Se desea estandarizar variables de distinta magnitud, sin introducir funciones no lineales.
* Se requiere una alternativa rápida y eficiente para centrar y escalar datos cuando no hay muchos valores atípicos.
* Se trabaja con algoritmos sensibles al centrado de datos, como redes neuronales, regresiones, PCA, etc.

### Supuestos:

* No requiere normalidad, ni distribución específica.
* Se asume que el rango (máx - mín) representa adecuadamente la dispersión de los datos (ojo con outliers).
* Puede perder robustez si hay valores extremos que distorsionen el rango.

---

## Decimal Scaling

### Descripción:

El escalado por desplazamiento decimal transforma los datos dividiéndolos entre una potencia de 10 que garantiza que el valor absoluto máximo de los datos transformados esté dentro del intervalo $[-1, 1]$.

La idea principal es encontrar cuántos dígitos se necesitan para "desplazar" la magnitud del número hacia un rango menor y más manejable. Este método es muy sencillo y eficiente computacionalmente, y resulta ideal en contextos donde se prioriza la simplicidad matemática y la preservación de la forma de los datos.

### Formulación:

$$x'_i = \frac{x_i}{10^j}$$

* Donde:

    * $x'_i$: es el valor transformado.
    * $x_i$: es el valor original.
    * $j$: es el mínimo entero tal que:
      $$\max(|x'_i|) < 1$$
      En la práctica, $j$ se calcula como:
      $$j = \lceil \log_{10}(\max(|x_i|)) \rceil$$

### Beneficios:

* Fácil de entender e implementar sin necesidad de estadísticos como media o desviación.
* Útil para modelos simples o sistemas embebidos con capacidad de cómputo limitada.
* Los datos escalados mantienen una interpretación proporcional directa.
* No distorsiona la forma original de la distribución.

### Caso de Uso:

* Se requiere una transformación simple, rápida y sin supuestos estadísticos.
* Se necesita que los valores queden acotados entre -1 y 1, sin alterar sus proporciones.
* Se trabaja en contextos educativos o didácticos donde se enseñan fundamentos del preprocesamiento.
* El sistema de cómputo no tolera números muy grandes (por ejemplo, sensores o sistemas de control).

### Supuestos:

* No hay supuestos estadísticos, pero se asume que el valor máximo absoluto representa bien la escala de los datos.
* Si hay outliers muy grandes, el escalado puede ser demasiado drástico (es decir, todos los demás valores pueden quedar muy cercanos a 0).

---

## Box-Cox

### Descripción:

El Box-Cox es más que un simple método de escalamiento; es una transformación de potencia paramétrica que busca hacer que los datos se asemejen a una distribución normal. A diferencia de los métodos tradicionales de escalado, Box-Cox ajusta dinámicamente la transformación en función de los datos, utilizando un parámetro llamado $\lambda$ (lambda), que se elige automáticamente para maximizar la normalidad de la variable transformada.

Este método es ideal cuando los datos presentan asimetría o heterocedasticidad, y se requiere una transformación que mejore la linealidad o reduzca la varianza antes de aplicar modelos estadísticos o machine learning.

### Formulación:

$$x'_i =
\begin{cases}
\frac{x_i^\lambda - 1}{\lambda}, & \text{si } \lambda \ne 0 \\
\log(x_i), & \text{si } \lambda = 0
\end{cases}$$

* Donde:

    * $x'_i$: es el valor transformado.
    * $x_i$: es el valor original (debe ser positivo estrictamente).
    * $\lambda$: es el parámetro de transformación, elegido automáticamente (por ejemplo, vía máxima verosimilitud).

### Beneficios:

* Mejora la normalidad de la distribución, ideal para modelos que la asumen (por ejemplo, regresión lineal).
* Puede reducir la varianza y la heterocedasticidad, haciendo más fiables los análisis estadísticos.
* La transformación es reversible, y se puede deshacer si se guarda el valor de $λ$.
* Ajusta dinámicamente la transformación según la variable, sin necesidad de decidir manualmente la potencia.

### Caso de Uso:

* Los datos no están normalmente distribuidos y se desea una transformación que corrija la asimetría.
* Se requiere cumplir con los supuestos de normalidad o homocedasticidad en modelos como regresión o ANOVA.
* Se trabaja con algoritmos que se benefician de una distribución simétrica y centrada, como PCA o redes neuronales.
* Se desea mejorar la linealidad de relaciones entre variables.

### Supuestos:

* Todos los valores deben ser positivos estrictamente: si hay ceros o negativos, Box-Cox no puede aplicarse directamente.
* Se asume que existe una transformación de potencia que puede mejorar la normalidad de la variable.
* Puede no ser ideal para variables categóricas codificadas numéricamente o datos ya transformados.

---

## Yeo-Johnson

### Descripción:

La transformación Yeo-Johnson es una generalización de la transformación Box-Cox, diseñada para poder transformar también valores negativos y ceros, manteniendo la idea de "hacer que los datos se acerquen a una distribución normal". Es paramétrica y continua, lo que significa que ajusta automáticamente el tipo de transformación en función de la forma de los datos.

Es ideal para preprocesamiento en modelos lineales, regresión, clustering o cualquier técnica que se beneficie de datos con simetría, baja curtosis y homocedasticidad.

### Formulación:

La fórmula de Yeo-Johnson depende del signo de los datos y del parámetro $\lambda$:

Para $x \geq 0$:

$$x'_i =
\begin{cases}
\frac{(x_i + 1)^\lambda - 1}{\lambda} & \text{si } \lambda \ne 0 \\
\log(x_i + 1) & \text{si } \lambda = 0
\end{cases}$$

Para $x < 0$:

$$x'_i =
\begin{cases}
-\frac{(-x_i + 1)^{2 - \lambda} - 1}{2 - \lambda} & \text{si } \lambda \ne 2 \\
-\log(-x_i + 1) & \text{si } \lambda = 2
\end{cases}$$

* Donde:

    * $x_i$: es el valor original (puede ser negativo, cero o positivo).
    * $x'_i$: es el valor transformado.
    * $\lambda$: es un parámetro que se ajusta automáticamente para normalizar la variable.

### Beneficios:

* Acepta valores negativos, ceros y positivos, a diferencia de Box-Cox.
* Reduce la asimetría y mejora la normalidad de los datos.
* Facilita la aplicación de modelos lineales y estadísticos clásicos.
* Puede mejorar significativamente el rendimiento de modelos sensibles a la forma de la distribución.

### Caso de Uso:

* Tus variables no están distribuidas normalmente.
* Existen valores negativos o ceros que no se pueden eliminar o imputar.
* Vas a aplicar modelos como regresión lineal, regresión logística, análisis factorial, clustering, etc., y necesitas distribuciones más simétricas.
* Trabajas con datos financieros, biomédicos o sociales con asimetría típica y valores negativos.

### Supuestos:

* No requiere que los datos sean estrictamente positivos.
* No transforma datos categóricos; debe aplicarse solo a variables numéricas continuas.
* El ajuste del parámetro $\lambda$ se realiza para cada variable individualmente.

---

## Softmax

### Descripción:

La transformación Softmax convierte un conjunto de valores numéricos en una distribución de probabilidades. Es decir, transforma los valores de cada columna en valores positivos entre 0 y 1, y la suma de todos esos valores será igual a 1 por columna.

Aunque Softmax es comúnmente usada en la capa de salida de modelos de clasificación multiclase, también puede utilizarse como método de escalado cuando se desea transformar una variable numérica en una representación probabilística, útil para ciertos algoritmos probabilísticos, comparaciones relativas o métodos de ponderación.

### Formulación:

$$x'_i = \frac{e^{x_i}}{\sum_{j=1}^{n} e^{x_j}}$$

* Donde:

    * $x'_i$: es el valor escalado (resultado de Softmax).
    * $x_i$: es el valor original.
    * $e^{x_i}$: es la exponencial del valor original.
    * $\sum_{j=1}^{n} e^{x_j}$: es la suma de todas las exponenciales de los valores de la misma columna.

### Beneficios:

* Convierte los datos en valores comparables dentro de un contexto probabilístico.
* Muy útil para representar importancia relativa o probabilidades normalizadas.
* Hace que todos los valores sean positivos y acotados entre 0 y 1.
* Suma total igual a 1 por columna, útil en algoritmos que esperan distribuciones.

### Caso de Uso:

* Se desea transformar valores en distribuciones de probabilidad por columna.
* Se quiere destacar qué observaciones tienen valores relativamente más altos en comparación al resto.
* Se trabaja con ponderaciones, decisiones probabilísticas o selección por probabilidad.
* Se utilizan modelos que interpretan los datos como proporciones o probabilidades relativas.

### Supuestos:

* No hay supuestos estrictos, pero:
    * Softmax es muy sensible a valores extremos: un valor mucho mayor que el resto puede dominar la distribución.
    * No es una buena opción si se requiere preservar las diferencias absolutas entre observaciones (ya que se normaliza todo a una distribución relativa).
    * Requiere valores numéricos reales (float o int), no funciona con valores categóricos.

---

## Escalas Logarítmicas

### Descripción:

Este método consiste en aplicar una transformación logarítmica a los datos para reducir la asimetría y compresión de valores extremos, y luego aplicar una estandarización clásica (Z-score) para que los datos transformados tengan media cero y desviación estándar uno.

Es especialmente útil en variables que presentan distribuciones fuertemente sesgadas a la derecha (long tail) o que tienen rangos muy amplios de valores, como ingresos, visitas, conteos, etc. La transformación logarítmica "comprime" los valores grandes y amplifica los pequeños, ayudando a suavizar la distribución antes de normalizarla.

### Formulación:

Primero, se aplica la transformación logarítmica:

$$x^{(log)}_i = \log(x_i)$$

Luego, se aplica el escalado Z-score sobre los valores transformados:

$$x'_i = \frac{x^{(log)}_i - \mu_{log}}{\sigma_{log}}$$

* Donde:

    * $x_i$: es el valor original (positivo).
    * $x^{(log)}_i$: es el valor transformado con logaritmo.
    * $x'_i$: es el valor final escalado.
    * $\mu_{log}$: es la media de los valores logarítmicos.
    * $\sigma_{log}$: es la desviación estándar de los valores logarítmicos.

### Beneficios:

* Reduce la asimetría de variables sesgadas (especialmente sesgo positivo).
* Disminuye el impacto de los outliers al comprimir valores grandes.
* La estandarización posterior permite usar los datos en algoritmos sensibles a la escala, como SVM, KNN o PCA.
* Ideal para datos con crecimiento exponencial o muy dispersos.

### Caso de Uso:

* Los datos son positivos y presentan distribuciones sesgadas o con long tails.
* Se busca mejorar la normalidad de los datos antes de aplicar modelos estadísticos.
* Se trabaja con algoritmos que asumen distribuciones centradas y escaladas.
* Es necesario homogeneizar variables con distintos rangos y escalas.

### Supuestos:

* Todos los valores deben ser mayores que cero antes de aplicar el logaritmo.
* Se recomienda verificar que no existan ceros ni valores negativos.
* Es una técnica sensible a valores extremos antes del logaritmo si no se preprocesa adecuadamente.

---

## QuantileTransformer

### Descripción:

El QuantileTransformer es una técnica no paramétrica que transforma la distribución de una variable para que siga una distribución uniforme o normal (gaussiana), preservando el orden de los valores. Esto se logra asignando a cada valor un percentil basado en su ranking, y luego mapeando ese percentil a la nueva distribución deseada.

Esta técnica es especialmente útil para homogeneizar distribuciones, controlar outliers y estandarizar variables antes de usar modelos sensibles a la forma de la distribución.

### Formulación:

* Primero se calcula el ranking o percentil de cada valor:

$$q_i = \frac{\text{rango}(x_i)}{n}$$

* Luego se transforma ese percentil $q_i$ a una nueva distribución:

    * Para una distribución uniforme: $x'_i = q_i$
    * Para una distribución normal: $x'_i = \Phi^{-1}(q_i)$


* Donde:
    
    * $x_i$: es el valor original.
    * $q_i$: es el percentil estimado del valor.
    * $x'_i$: es el valor transformado.
    * $\Phi^{-1}$: es la función inversa de la CDF normal estándar (la función cuantílica de la normal).
    * $n$: es el número total de observaciones.
    * $\text{rango}(x_i)$: es la posición ordenada de $x_i$.

### Beneficios:

* No depende de parámetros estadísticos como media o desviación estándar.
* Permite transformar cualquier distribución hacia una forma completamente uniforme o normal.
* Reduce el impacto de outliers, ya que trabaja por percentiles.
* Útil en modelos como regresión lineal, PCA, o cualquier algoritmo que asuma normalidad.
* Preserva el orden relativo de los datos (monótona).

### Caso de Uso:

* Las variables tienen distribuciones no normales, con asimetrías fuertes o múltiples picos.
* Se quiere normalizar o uniformizar la variable antes de aplicar modelos sensibles a la distribución.
* Se necesita que los valores estén contenidos entre 0 y 1 (en el caso de uniformes).
* Se aplica PCA, análisis de componentes independientes o redes neuronales, donde normalidad mejora el desempeño.

### Supuestos:

* Se asume que los datos son continuos y numéricos.
* No transforma variables categóricas.
* Es sensible a los valores repetidos, ya que el ranking puede no ser único.

---

## L1 Normalization (Norma Manhattan)

### Descripción:

El escalado por Norma L1 transforma cada fila del conjunto de datos para que la suma absoluta de sus valores sea igual a 1. Esto se logra dividiendo cada componente de la fila por la suma total de los valores absolutos de esa misma fila.

Este tipo de escalado convierte cada fila en una especie de proporción relativa, permitiendo comparar vectores que pueden estar en diferentes escalas pero tienen formas similares.

Se le llama Norma Manhattan porque corresponde a la distancia $L_1$ entre puntos, es decir, la suma de los valores absolutos.

### Formulación:

$$x'_i = \frac{x_i}{\sum_{j=1}^d |x_j|}$$

* Donde:

    * $x_i$: valor original en la fila.
    * $x'_i$: valor escalado.
    * $|x_j|$: valor absoluto del elemento $j$-ésimo en la misma fila.
    * $d$: número de columnas (dimensiones) del vector.

### Beneficios:

* Transforma cada fila en una representación proporcional, útil cuando la magnitud absoluta no importa.
* Resalta las características dominantes dentro de cada observación.
* Muy útil en problemas de clasificación de texto, imágenes, y modelos donde el patrón interno de cada fila es más importante que sus escalas absolutas.
* Ideal para modelos que usan la distancia Manhattan o modelos lineales con regularización L1.

### Caso de Uso:

* Se usan algoritmos que trabajan con distancias entre vectores de observaciones, como KNN, SVM o clustering.
* Quieres que cada fila (observación) esté en la misma escala relativa, especialmente útil cuando los valores son frecuencias, conteos o pesos.
* Necesitas que los datos cumplan una condición de unidad en la norma L1.

### Supuestos:

* Se asume que los datos son numéricos.
* No debe usarse en variables categóricas codificadas numéricamente.
* Debe aplicarse solo si cada fila representa una entidad comparable como un vector.

---

## L2 Normalization (Norma Euclidiana)

### Descripción:

La L2 Normalization transforma cada fila del dataset para que su norma Euclidiana (longitud) sea igual a 1. Esto se logra dividiendo cada componente de la fila por la raíz cuadrada de la suma de los cuadrados de todos los valores de esa fila.

Este tipo de normalización preserva la dirección del vector pero cambia su magnitud a 1, lo que permite comparar vectores independientemente de su escala original. Es fundamental en modelos donde la similitud angular o la longitud del vector afecta el resultado, como en clasificación de texto, sistemas de recomendación o embeddings.

### Formulación:

$$x'_i = \frac{x_i}{\sqrt{\sum_{j=1}^d x_j^2}}$$

* Donde:

    * $x_i$: valor original en la fila.
    * $x'_i$: valor normalizado.
    * $x_j$: valor del elemento $j$-ésimo en la misma fila.
    * $d$: número de dimensiones (columnas).
    * $\sum_{j=1}^d x_j^2$: suma de los cuadrados de los elementos de la fila (norma L2 al cuadrado).

### Beneficios:

* Cada fila pasa a tener longitud unitaria (norma 1).
* Preserva la orientación del vector, eliminando el efecto de la magnitud.
* Muy útil en tareas de análisis de similitud, clustering vectorial, y modelos que emplean productos escalares.
* Favorece la convergencia numérica en redes neuronales.
* Reduce la dominancia de variables con valores grandes.

### Caso de Uso:

* Se usan modelos que calculan distancias o productos escalares entre vectores (como KNN, SVM, PCA).
* La magnitud de las observaciones no es relevante, solo su dirección relativa.
* Se aplican algoritmos de aprendizaje automático en datos de texto (TF-IDF, word embeddings).
* Se quiere reducir la sensibilidad a escalas absolutas entre observaciones.

### Supuestos:

* Los datos deben ser numéricos y continuos.
* Cada fila debe representar un vector que puede ser interpretado en el espacio.
* No debe aplicarse sobre variables categóricas codificadas numéricamente.

---

## MaxAbsScaler

### Descripción:

El MaxAbsScaler escala cada variable dividiendo sus valores por el valor absoluto máximo de esa columna, de modo que el resultado queda siempre en el rango [-1, 1]. Es una transformación lineal que no centra los datos (no resta la media), por lo que preserva los ceros y la dispersión original.

Esto lo hace ideal para casos donde necesitas mantener la estructura de ceros, como en vectores dispersos (sparse), y al mismo tiempo llevar todas las columnas a una escala común para que no dominen unas sobre otras.

### Formulación:

$$x'_i = \frac{x_i}{\max(|x|)}$$

* Donde:

    * $x_i$: valor original.
    * $x'_i$: valor transformado.
    * $\max(|x|)$: el valor máximo absoluto de la columna.

### Beneficios:

* Escala en el rango [-1, 1] sin desplazar los datos (no afecta los ceros).
* Ideal para datos dispersos (sparse) ya que no destruye la estructura.
* Sencillo, eficiente y sin suposiciones estadísticas.
* Conserva la forma de la distribución original de los datos.
* Útil en modelos lineales, redes neuronales, y métodos basados en distancias.

### Caso de Uso:

* Trabajas con vectores dispersos como representaciones TF-IDF, matrices de recomendación, conteos de palabras, etc.
* No quieres afectar los ceros en los datos (evitando centrado).
* Necesitas llevar los datos a una escala común para evitar dominancia por variables más grandes.
* Requieres mantener la orientación geométrica de los datos.

### Supuestos:

* Solo se asume que los datos son numéricos.
* No requiere normalidad ni continuidad.
* No transforma la distribución original, solo su escala.

---

## Sigmoid Scaling

### Descripción:

El Sigmoid Scaling aplica la función sigmoide a cada valor del conjunto de datos. Esta función tiene una forma característica de "S" y transforma cualquier número real a un valor en el rango (0, 1).

A diferencia de otros métodos lineales como Z-score o Min-Max, este escalado comprime los valores extremos hacia los bordes del rango y expande los valores cercanos al cero, siendo especialmente útil cuando se desea reducir el impacto de los outliers sin eliminarlos.

Es común en redes neuronales como función de activación, pero también puede usarse como preprocesamiento de datos.

*Nota adicional: Si los valores originales son muy grandes o muy pequeños, puede haber problemas de overflow numérico. Para evitarlo, es útil primero escalar con Z-score y luego aplicar el sigmoid (opcional).*

### Formulación:

$$x'_i = \frac{1}{1 + e^{-x_i}}$$

* Donde:

    * $x_i$: valor original.
    * $x'_i$: valor transformado mediante la función sigmoide.
    * $e$: la base del logaritmo natural, aproximadamente 2.718.

### Beneficios:

* Lleva todos los valores al rango (0, 1).
* Reduce la influencia de outliers, especialmente valores muy grandes o muy pequeños.
* No requiere información previa (como media o varianza).
* Útil en modelos donde se requiere compresión de valores sin perder su continuidad.
* Aplicable a modelos neurales o probabilísticos, donde se interpretan como "probabilidades".

### Caso de Uso:

* Se quiere reducir el efecto de valores extremos sin hacer cortes abruptos.
* Se trabaja con algoritmos sensibles a magnitudes grandes.
* Se desea usar el resultado como input de una red neuronal o como una probabilidad.
* No es necesario conservar la linealidad original de los datos.

### Supuestos:

* Aplica solo a datos numéricos.
* No requiere normalidad ni supuestos distribucionales.
* No debe aplicarse directamente a datos ya normalizados entre 0 y 1, ya que puede "aplastar" aún más su rango.

---

## RankUniform

### Descripción:

El RankUniform Scaling transforma los datos ordenándolos (ranking) y luego asignándoles valores distribuidos uniformemente en el rango [0,1]. Garantiza comparabilidad de variables independientemente de la distribución original.

En otras palabras: toma tus datos, los ordena del menor al mayor, les asigna un percentil y luego lo transforma al equivalente en una distribución uniforme. Este tipo de transformación es útil cuando quieres que todas las variables tengan la misma escala y rango, sin asumir nada sobre la forma original de la distribución.

*Nota final: Este método es muy usado en preprocesamiento de datos para redes neuronales, SVM o clustering, donde la escala uniforme ayuda a la convergencia y estabilidad.*

### Formulación:

Aunque no hay una fórmula exacta única (es un proceso), se puede entender como:

$$x'_i = \frac{r_i - 0.5}{n}$$

* Donde:

    * $x'_i$: valor escalado.  
    * $r_i$: rango (posición ordenada) del dato $x_i$.  
    * $n$: número total de observaciones.  

### Beneficios:

* Escala cualquier variable a un rango [0,1].  
* Robusto frente a outliers.  
* Hace variables comparables entre sí.  
* Ideal para modelos sensibles a escala como redes neuronales o SVM.  

### Caso de Uso:

* Normalizar múltiples variables a un rango común.  
* Modelos que requieren entrada entre 0 y 1.  
* Datos sesgados o con outliers.  
* Preprocesamiento para clustering, PCA o redes neuronales.  

### Supuestos:

* Solo requiere que los datos sean ordenables numéricamente.  
* No requiere distribución específica ni parámetros previos.  
* No es recomendable si necesitas preservar relaciones originales entre las variables (puede distorsionar interpretaciones si la escala es importante).

---

## Discretización (KBinsDiscretizer)

### Descripción:

La discretización convierte variables numéricas continuas en categorías discretas o "bins". Es decir, transforma los valores originales en intervalos, etiquetándolos por el número de bin al que pertenecen.

El método KBinsDiscretizer permite discretizar usando tres estrategias diferentes:

* Uniforme ('uniform'): divide el rango en bins de igual ancho.
* Cuantiles ('quantile'): divide de forma que cada bin tenga aproximadamente el mismo número de observaciones.
* K-means ('kmeans'): agrupa valores en bins minimizando la varianza intra-bin, como un clustering.

Puedes elegir codificar los resultados como etiquetas ordinales (0,1,2...) o como variables dummies (one-hot).

*Tip: Si usas árboles de decisión o random forests, es mejor usar encode='ordinal'. Si vas a usar regresión logística o redes, onehot-dense es más recomendable.*

### Formulación:

No hay una fórmula directa porque depende del método elegido, pero conceptualmente se define como:

$$x'_i = \text{bin}(x_i)$$

* Donde:

    * $x_i$: valor original.
    * $x'_i$: número de bin o vector one-hot correspondiente al intervalo al que pertenece $x_i$.

### Beneficios:

* Convierte variables continuas en categóricas discretas, facilitando el uso en algoritmos como Naive Bayes o árboles de decisión.
* Reduce la sensibilidad a pequeñas fluctuaciones en los datos.
* Mejora la interpretabilidad al dividir los datos en rangos.
* Puede capturar no linealidades de forma simple.

### Caso de Uso:

* Quieres reducir la cardinalidad de variables continuas.
* Trabajas con algoritmos que funcionan mejor con variables categóricas.
* Necesitas crear features interpretables (por ejemplo, edad en rangos).
* Deseas aplicar ingeniería de atributos basada en grupos.

### Supuestos:

* Requiere datos numéricos.
* Si se usa strategy='quantile', los datos deben tener observaciones suficientes para dividir equitativamente.
* En strategy='kmeans', es sensible a la escala, así que se recomienda escalar previamente.

---

## Logit Scaling

### Descripción:

El Logit Scaling transforma variables acotadas en el rango [0,1] usando la función logit:

$$x'_i = \log\left(\frac{x_i}{1-x_i}\right)$$

Convierte los valores de la variable en un rango de $(-\infty, +\infty)$, expandiendo los extremos y comprimiendo los valores cercanos a 0.5. Esta transformación es útil cuando se quiere estabilizar la varianza de proporciones o probabilidades, y facilita que los modelos lineales interpreten mejor las relaciones no lineales en variables acotadas.

*Nota final: Esta técnica es muy utilizada en modelamiento de probabilidades y proporciones, especialmente cuando los datos están cerca de los límites 0 o 1.*

### Formulación:

$$x'_i = \log\left(\frac{x_i}{1-x_i}\right)$$

* Donde:

    * $x'_i$: valor escalado.  
    * $x_i$: valor original de la variable, debe estar en (0,1).  

### Beneficios:

* Convierte datos acotados a un rango ilimitado.  
* Amplifica diferencias cerca de los extremos (0 y 1).  
* Facilita la modelación lineal de probabilidades o proporciones.  
* Útil para estabilizar la varianza y mejorar convergencia de modelos.  

### Caso de Uso:

* Variables que representan proporciones o probabilidades.  
* Preprocesamiento para regresión logística u otros modelos lineales.  
* Cuando los datos están cerca de los límites 0 o 1 y quieres diferenciarlos mejor.  

### Supuestos:

* Todos los valores deben estar estrictamente entre 0 y 1.  
* No aplicar si existen ceros o unos, a menos que se recodifiquen levemente (ej. $x = 0 \to 0.0001$, $x = 1 \to 0.9999$).  
* Solo para variables numéricas acotadas.

---

## Winsorization / Capping de Extremos

### Descripción:

La técnica de Winsorization consiste en **recortar los valores extremos** de una variable para limitar el efecto de outliers. En lugar de eliminar los valores atípicos, se reemplazan por percentiles específicos (por ejemplo, el 5° y 95° percentil). Esto ayuda a estabilizar la varianza y a mejorar la robustez de los modelos ante valores extremos que podrían sesgar los resultados.

*Nota final: Muy utilizada en finanzas, métricas de desempeño y cualquier dataset con valores atípicos que podrían distorsionar escalados o análisis.*

### Formulación:

Sea $x_i$ un valor de la variable y $p_{low}$ y $p_{high}$ los percentiles inferior y superior deseados:

$$
x'_i =
\begin{cases} 
p_{low}, & \text{si } x_i < p_{low} \\
x_i, & \text{si } p_{low} \le x_i \le p_{high} \\
p_{high}, & \text{si } x_i > p_{high}
\end{cases}
$$

* Donde:

    * $x'_i$: valor escalado tras Winsorization.  
    * $p_{low}$: percentil inferior (por ejemplo, 5°).  
    * $p_{high}$: percentil superior (por ejemplo, 95°).  

### Beneficios:

* Reduce el impacto de outliers sin eliminarlos.  
* Mejora la robustez de estadísticas como media, desviación estándar y correlación.  
* Facilita la aplicación de técnicas de escalado posteriores (Z-score, MinMax, Logit, etc.).  
* Preserva la mayoría de la información de la distribución.  

### Caso de Uso:

* Datos financieros con valores extremos (ingresos, precios, ratios).  
* Métricas de rendimiento donde los valores atípicos pueden sesgar modelos.  
* Preprocesamiento previo a escalados o modelos sensibles a outliers.  

### Supuestos:

* La elección de los percentiles debe ser consistente con el contexto del negocio.  
* Solo aplica a variables numéricas.  
* No sustituye análisis de outliers, solo limita su efecto.

---

## VIF-based Scaling (Reducción de Multicolinealidad)

### Descripción:

El escalado basado en VIF (Variance Inflation Factor) tiene como objetivo **reducir el impacto de multicolinealidad** en las variables. La idea es que las variables muy correlacionadas entre sí pueden inflar la varianza de los coeficientes en modelos lineales, afectando la interpretación y estabilidad del modelo. Al escalar cada variable por su VIF, se atenúa el efecto de variables redundantes y se mejora la robustez del modelado.

*Nota final: Este método es especialmente útil en regresión lineal, regresión logística y cualquier análisis que dependa de estimaciones precisas de coeficientes.*

### Formulación:

Sea $X_j$ la variable j-ésima de un dataset con $d$ variables y $VIF_j$ su factor de inflación de varianza:

1. Calcular $VIF_j$ para cada variable:

$$
VIF_j = \frac{1}{1 - R_j^2}
$$

donde $R_j^2$ es el coeficiente de determinación de la regresión de $X_j$ sobre las demás variables.

2. Escalar la variable:

$$
X'_j = \frac{X_j}{VIF_j}
$$

* Donde:

    * $X'_j$: valor escalado.  
    * $VIF_j$: factor de inflación de varianza de la variable j.  

### Beneficios:

* Reduce la multicolinealidad entre variables.  
* Mejora la estabilidad y interpretabilidad de modelos lineales.  
* Mantiene la mayoría de la información original de las variables.  
* Permite combinar variables con diferentes niveles de correlación sin sesgar el modelo.  

### Caso de Uso:

* Regresiones lineales o logísticas con variables numéricas altamente correlacionadas.  
* Modelos donde se requiere interpretación de coeficientes o contribuciones de cada variable.  
* Preprocesamiento antes de aplicar técnicas sensibles a colinealidad.  

### Supuestos:

* Solo aplica a variables numéricas.  
* VIF debe calcularse sobre un dataset con al menos dos columnas (no funciona con una sola variable).  
* No reemplaza análisis profundo de correlaciones o reducción de dimensionalidad (como PCA) si es necesario.

---

## WOE (Weight of Evidence) Transformation

### Descripción:

La transformación WOE convierte variables categóricas o continuas discretizadas en valores que reflejan la fuerza y dirección de la relación con un **evento binario** (por ejemplo, default/no default, churn/no churn). Es ampliamente usada en **scoring crediticio y modelos de riesgo**. Cada categoría o bin se transforma según la proporción de eventos y no eventos, facilitando la interpretación y linealidad con la variable objetivo.

*Nota final: WOE es especialmente útil cuando se combinan varias variables para construir un score lineal, como en regresión logística para riesgo crediticio.*

### Formulación:

Para cada bin o categoría $i$:

$$
WOE_i = \ln \left( \frac{P(\text{No Evento}_i)}{P(\text{Evento}_i)} \right) = \ln \left( \frac{\text{Good}_i / \text{Total Good}}{\text{Bad}_i / \text{Total Bad}} \right)
$$

* Donde:

- $P(\text{Evento}_i)$: proporción de eventos en el bin i.  
- $P(\text{No Evento}_i)$: proporción de no eventos en el bin i.  
- $WOE_i$: valor transformado para ese bin.  

### Beneficios:

* Facilita la **linealidad** de la variable con el logit en regresión logística.  
* Reduce el efecto de categorías con pocas observaciones.  
* Maneja variables categóricas y discretizadas de forma consistente.  
* Mejora interpretabilidad y permite calcular el **Information Value (IV)** para seleccionar variables relevantes.  

### Caso de Uso:

* Modelos de riesgo financiero, scoring crediticio, predicción de churn.  
* Variables categóricas o continuas discretizadas en bins.  
* Preprocesamiento antes de regresión logística o modelos lineales.  

### Supuestos:

* Funciona mejor con variables que tienen relación monotónica con la variable objetivo.  
* Requiere variable objetivo binaria.  
* No es recomendable para variables continuas sin discretizar o sin relación con la variable objetivo.  

---

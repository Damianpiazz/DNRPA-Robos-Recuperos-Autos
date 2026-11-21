# Guía Práctica y Formulario de Mínimos Cuadrados y Regresión

*Basado en los métodos y la rigorometría matemática de Walpole, Myers, Myers & Ye ("Probabilidad y Estadística para Ingeniería y Ciencias")*

---

## 1. Fundamentos del Modelo de Regresión según Walpole

En el análisis de datos para ingeniería y ciencias, se busca modelar la relación funcional entre una variable de respuesta cuantitativa $Y$ y una o más variables independientes o regresoras $x$. Walpole establece una distinción tajante entre las relaciones deterministas (donde una variable determina de forma exacta a otra) y las **relaciones probabilísticas o empíricas**, en las cuales existe variabilidad no explicada.

### 1.1 Formulación Estadística del Modelo de Regresión Lineal Simple (RLS)
Para un conjunto de $n$ pares muestrales observados $\{(x_i, y_i); i = 1, 2, \dots, n\}$, la respuesta observada $y_i$ se expresa como la suma de un componente determinista lineal y un componente aleatorio $e_i$ o $\epsilon_i$:

$$Y_i = \beta_0 + \beta_1 x_i + \epsilon_i$$

donde:
- $\beta_0$ es la intersección poblacional con el eje de ordenadas (constante del modelo).
- $\beta_1$ es la pendiente poblacional, que representa el cambio en la respuesta media por unidad de cambio en $x$.
- $\epsilon_i$ es la alteración o error aleatorio inobservable asociado a la $i$-ésima medición.

### 1.2 Supuestos del Modelo de Gauss-Markov y Normalidad
Para desarrollar la inferencia estadística de mínimos cuadrados, Walpole establece los siguientes supuestos fundamentales sobre las perturbaciones aleatorias $\epsilon_i$:

1. **Esperanza Nula**: $E(\epsilon_i) = 0$ para todo $i$. Esto implica que la media poblacional de $Y$ dado $x$ yace exactamente sobre la recta de regresión verdadera:
   $$E(Y | x) = \mu_{Y|x} = \beta_0 + \beta_1 x$$
2. **Homocedasticidad (Varianza Homogénea)**: $\text{Var}(\epsilon_i) = \text{Var}(Y_i | x_i) = \sigma^2$ constante para todo $i$.
3. **Independencia Estocástica**: $\text{Cov}(\epsilon_i, \epsilon_j) = 0$ para todo $i \neq j$. Los errores de observaciones distintas no están correlacionados.
4. **Normalidad**: Para la prueba de hipótesis e intervalos de confianza, se asume que los errores se distribuyen normalmente: $\epsilon_i \sim N(0, \sigma^2)$, lo que implica $Y_i \sim N(\beta_0 + \beta_1 x_i, \sigma^2)$.

### 1.3 Distinción entre Parámetros Poblacionales y Estimadores Muestrales
En la práctica, la recta de regresión verdadera $\mu_{Y|x} = \beta_0 + \beta_1 x$ es inobservable. A partir de los datos muestrales se estima la **recta de regresión ajustada**:

$$\hat{y} = b_0 + b_1 x$$

Walpole enfatiza la diferencia conceptual entre el **error poblacional inobservable** ($\epsilon_i = y_i - (\beta_0 + \beta_1 x_i)$) y el **residual muestral observable** ($e_i$):

$$e_i = y_i - \hat{y}_i = y_i - (b_0 + b_1 x_i)$$

El residual $e_i$ representa la desviación vertical entre el punto experimental medido $(x_i, y_i)$ y la recta ajustada en el gráfico de dispersión.

---

## 2. Regresión Lineal Simple: Formulario Computacional Abreviado

El método de mínimos cuadrados busca seleccionar los coeficientes $b_0$ y $b_1$ de modo que se minimice la Suma de Cuadrados del Error ($\text{SCE}$):

$$\text{SCE} = \sum_{i=1}^n e_i^2 = \sum_{i=1}^n (y_i - b_0 - b_1 x_i)^2$$

### 2.1 Ecuaciones Normales y Deducción Algebraica
Al calcular las derivadas parciales de $\text{SCE}$ con respecto a $b_0$ y $b_1$ e igualarlas a cero:

$$\frac{\partial (\text{SCE})}{\partial b_0} = -2 \sum_{i=1}^n (y_i - b_0 - b_1 x_i) = 0$$

$$\frac{\partial (\text{SCE})}{\partial b_1} = -2 \sum_{i=1}^n x_i (y_i - b_0 - b_1 x_i) = 0$$

Se obtiene el **Sistema de Ecuaciones Normales**:

$$n b_0 + b_1 \sum_{i=1}^n x_i = \sum_{i=1}^n y_i$$

$$b_0 \sum_{i=1}^n x_i + b_1 \sum_{i=1}^n x_i^2 = \sum_{i=1}^n x_i y_i$$

### 2.2 Notación de Sumas de Cuadrados de Walpole ($S_{xx}, S_{yy}, S_{xy}$)
Para simplificar significativamente el cálculo manual y programático, Walpole introduce las siguientes identidades abreviadas para las sumas de cuadrados y productos cruzados centrados:

$$S_{xx} = \sum_{i=1}^n (x_i - \bar{x})^2 = \sum_{i=1}^n x_i^2 - \frac{\left(\sum_{i=1}^n x_i\right)^2}{n}$$

$$S_{yy} = \sum_{i=1}^n (y_i - \bar{y})^2 = \sum_{i=1}^n y_i^2 - \frac{\left(\sum_{i=1}^n y_i\right)^2}{n}$$

$$S_{xy} = \sum_{i=1}^n (x_i - \bar{x})(y_i - \bar{y}) = \sum_{i=1}^n x_i y_i - \frac{\left(\sum_{i=1}^n x_i\right)\left(\sum_{i=1}^n y_i\right)}{n}$$

### 2.3 Fórmulas Directas de Cálculo
Resolviendo el sistema de ecuaciones normales expresado mediante las sumas abreviadas, se obtienen las expresiones directas para la pendiente $b_1$ y la intersección $b_0$:

$$b_1 = \frac{S_{xy}}{S_{xx}} = \frac{n \sum_{i=1}^n x_i y_i - \left(\sum_{i=1}^n x_i\right)\left(\sum_{i=1}^n y_i\right)}{n \sum_{i=1}^n x_i^2 - \left(\sum_{i=1}^n x_i\right)^2}$$

$$b_0 = \bar{y} - b_1 \bar{x} = \frac{\sum_{i=1}^n y_i - b_1 \sum_{i=1}^n x_i}{n}$$

### 2.4 Algoritmo Paso a Paso para Ajustar una RLS
Para resolver rápidamente cualquier problema de RLS a mano o en planilla de cálculo, se sigue este procedimiento estructurado:

1. **Construir la Tabla de Tabulación Muestral**: Formar 5 columnas para los $n$ datos observados:
   - Columna 1: $x_i$
   - Columna 2: $y_i$
   - Columna 3: $x_i^2$
   - Columna 4: $y_i^2$
   - Columna 5: $x_i y_i$
2. **Obtener las Sumatorias**: Sumar cada columna para hallar $\sum x_i$, $\sum y_i$, $\sum x_i^2$, $\sum y_i^2$, y $\sum x_i y_i$.
3. **Calcular Medias Muestrales**: $\bar{x} = \frac{\sum x_i}{n}$ y $\bar{y} = \frac{\sum y_i}{n}$.
4. **Calcular Sumas Centradas**: Evaluar $S_{xx}$, $S_{yy}$, y $S_{xy}$.
5. **Calcular Parámetros de la Recta**: Evaluar la pendiente $b_1 = \frac{S_{xy}}{S_{xx}}$ y luego la intersección $b_0 = \bar{y} - b_1 \bar{x}$.
6. **Escribir la Ecuación Muestral**: Presentar el modelo ajustado $\hat{y} = b_0 + b_1 x$.

---

## 3. Compendio de Transformaciones y Linealización de Modelos No Lineales

En diversas aplicaciones de ingeniería y ciencias físicas, las leyes teóricas subyacentes implican funciones no lineales. Walpole señala que cuando una función no lineal puede expresarse de modo que sea **lineal respecto a sus parámetros**, es posible linealizar los datos mediante una transformación matemática previa y aplicar el método de mínimos cuadrados ordinario.

### 3.1 Cheat-Sheet de Transformaciones de Walpole
La siguiente tabla resume las funciones de ajuste no lineales más comunes, la transformación requerida en las variables de trabajo $(x^*, y^*)$, el sistema de regresión resultante y la fórmula para recuperar los parámetros originales del modelo empírico.

| Modelo Original ($y = f(x)$) | Transformación de Variables | Modelo Linealizado Ajustado | Parámetros Inversos a Recuperar |
| :--- | :--- | :--- | :--- |
| **Exponencial**: $y = a e^{b x}$ | $y^* = \ln(y)$, $x^* = x$ | $y^* = a_0 + a_1 x^*$ | $a = e^{a_0}$, $b = a_1$ |
| **Potencial**: $y = a x^b$ | $y^* = \ln(y)$, $x^* = \ln(x)$ | $y^* = a_0 + a_1 x^*$ | $a = e^{a_0}$, $b = a_1$ |
| **Recíproco**: $y = a + b \left(\frac{1}{x}\right)$ | $y^* = y$, $x^* = \frac{1}{x}$ | $y^* = a_0 + a_1 x^*$ | $a = a_0$, $b = a_1$ |
| **Hiperbólico / Cociente**: $y = \frac{x}{a + b x}$ | $y^* = \frac{1}{y}$, $x^* = \frac{1}{x}$ | $y^* = a_0 + a_1 x^*$ | $a = a_1$, $b = a_0$ |
| **Crecimiento Saturado**: $y = \frac{a x}{b + x}$ | $y^* = \frac{1}{y}$, $x^* = \frac{1}{x}$ | $y^* = a_0 + a_1 x^*$ | $a = \frac{1}{a_0}$, $b = \frac{a_1}{a_0}$ |

### 3.2 Implicación Metodológica sobre la Estructura del Error
Un aspecto crítico resaltado por Walpole en la Sección 11.10 radica en que al transformar los datos (por ejemplo, aplicando la transformación logarítmica $\ln(y)$), se está alterando la estructura del error aleatorio subyacente.

- Si el modelo transformado posee un **error aditivo** $\ln(y_i) = \ln(a) + b x_i + \epsilon_i^*$, esto equivale a postular un **modelo de error multiplicativo** en las variables naturales:
  $$y_i = a e^{b x_i} \cdot \epsilon_i$$
  donde $\epsilon_i^* = \ln(\epsilon_i)$.
- La minimización por mínimos cuadrados en el espacio transformado minimiza la suma de cuadrados de las desviaciones relativas/logarítmicas $\sum (\ln y_i - \ln \hat{y}_i)^2$, en lugar de las desviaciones absolutas primarias en $y$.

---

## 4. Regresión Múltiple y Polinómica en Formato Matricial

Cuando intervienen $k$ variables regresoras $x_1, x_2, \dots, x_k$, o cuando se ajusta un polinomio de grado $k$ sobre una sola variable $x$, la formulación matricial desarrollada en el Capítulo 12 de Walpole provee el marco computacional uniforme y compacto.

### 4.1 Formulación Vectorial del Modelo General
El modelo de regresión lineal múltiple para $n$ observaciones se escribe como:

$$y_i = \beta_0 + \beta_1 x_{1i} + \beta_2 x_{2i} + \dots + \beta_k x_{ki} + \epsilon_i, \quad i = 1, 2, \dots, n$$

En notación matricial compacta:

$$\mathbf{y} = \mathbf{X} \boldsymbol{\beta} + \boldsymbol{\epsilon}$$

donde:

$$\mathbf{y} = \begin{bmatrix} y_1 \\ y_2 \\ \vdots \\ y_n \end{bmatrix}, \quad \mathbf{X} = \begin{bmatrix} 1 & x_{11} & x_{21} & \dots & x_{k1} \\ 1 & x_{12} & x_{22} & \dots & x_{k2} \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ 1 & x_{1n} & x_{2n} & \dots & x_{kn} \end{bmatrix}, \quad \boldsymbol{\beta} = \begin{bmatrix} \beta_0 \\ \beta_1 \\ \vdots \\ \beta_k \end{bmatrix}, \quad \boldsymbol{\epsilon} = \begin{bmatrix} \epsilon_1 \\ \epsilon_2 \\ \vdots \\ \epsilon_n \end{bmatrix}$$

### 4.2 Matriz de Diseño $\mathbf{X}$ para Regresión Polinómica
Para un ajuste polinómico de grado $k$ sobre una variable $x$ ($y = \beta_0 + \beta_1 x + \beta_2 x^2 + \dots + \beta_k x^k + \epsilon$), la matriz de diseño $\mathbf{X}$ adopta la estructura de una matriz de Vandermonde modificada:

$$\mathbf{X} = \begin{bmatrix} 1 & x_1 & x_1^2 & \dots & x_1^k \\ 1 & x_2 & x_2^2 & \dots & x_2^k \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ 1 & x_n & x_n^2 & \dots & x_n^k \end{bmatrix}$$

### 4.3 Ecuaciones Normales Matriciales y Solución
Minimizar la suma de cuadrados del error $\text{SCE} = (\mathbf{y} - \mathbf{X}\mathbf{b})'(\mathbf{y} - \mathbf{X}\mathbf{b})$ conduce al sistema de ecuaciones normales matriciales:

$$(\mathbf{X}'\mathbf{X}) \mathbf{b} = \mathbf{X}'\mathbf{y}$$

Donde la matriz $(\mathbf{X}'\mathbf{X})$ de dimensión $(k+1) \times (k+1)$ contiene los productos escalares de las columnas de $\mathbf{X}$:

$$\mathbf{X}'\mathbf{X} = \begin{bmatrix} n & \sum x_{1i} & \sum x_{2i} & \dots & \sum x_{ki} \\ \sum x_{1i} & \sum x_{1i}^2 & \sum x_{1i}x_{2i} & \dots & \sum x_{1i}x_{ki} \\ \sum x_{2i} & \sum x_{1i}x_{2i} & \sum x_{2i}^2 & \dots & \sum x_{2i}x_{ki} \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ \sum x_{ki} & \sum x_{1i}x_{ki} & \sum x_{2i}x_{ki} & \dots & \sum x_{ki}^2 \end{bmatrix}, \quad \mathbf{X}'\mathbf{y} = \begin{bmatrix} \sum y_i \\ \sum x_{1i}y_i \\ \sum x_{2i}y_i \\ \vdots \\ \sum x_{ki}y_i \end{bmatrix}$$

Si la matriz $(\mathbf{X}'\mathbf{X})$ es de rango completo (no singular), existe su inversa y el vector de estimadores de mínimos cuadrados $\mathbf{b}$ está dado de forma única por:

$$\mathbf{b} = (\mathbf{X}'\mathbf{X})^{-1} \mathbf{X}'\mathbf{y}$$

### 4.4 Matriz de Varianza-Covarianza de los Coeficientes
Si se cumplen los supuestos de Gauss-Markov con errores homocedásticos e independientes ($\text{Var}(\boldsymbol{\epsilon}) = \sigma^2 \mathbf{I}_n$), el vector estimador $\mathbf{b}$ es insesgado ($E(\mathbf{b}) = \boldsymbol{\beta}$) y su matriz de varianza-covarianza es:

$$\text{Var}(\mathbf{b}) = \sigma^2 (\mathbf{X}'\mathbf{X})^{-1}$$

Los elementos en la diagonal principal de $(\mathbf{X}'\mathbf{X})^{-1}$, denotados como $c_{jj}$, multiplicados por la varianza residual estimada $s^2$, proveen las varianzas muestrales de los coeficientes: $\text{Var}(b_j) = s^2 c_{jj}$.

---

## 5. Evaluación de la Bondad de Ajuste e Inferencia Estadística

Una vez obtenido el modelo ajustado, resulta imprescindible cuantificar la precisión del ajuste, medir el grado de asociación entre variables y realizar inferencias sobre los parámetros.

### 5.1 Estimación de la Varianza del Error ($s^2$)
Walpole define el **Error Cuadrático Medio ($s^2$)** como el estimador insesgado de la varianza del error $\sigma^2$:

$$s^2 = \text{CME} = \frac{\text{SCE}}{n - k - 1}$$

donde $k$ es el número de regresores (en RLS $k=1$, por lo que el divisor es $n-2$).

La Suma de Cuadrados del Error se calcula eficientemente mediante las identidades:
- Para Regresión Lineal Simple:
  $$\text{SCE} = S_{yy} - b_1 S_{xy} = S_{yy} - \frac{S_{xy}^2}{S_{xx}}$$
- Para Regresión Múltiple / Matricial:
  $$\text{SCE} = \mathbf{y}'\mathbf{y} - \mathbf{b}'\mathbf{X}'\mathbf{y}$$

### 5.2 Tabla de Análisis de Varianza (ANOVA)
La variabilidad total observada en la variable de respuesta se descompone según la identidad fundamental de la suma de cuadrados:

$$\sum_{i=1}^n (y_i - \bar{y})^2 = \sum_{i=1}^n (\hat{y}_i - \bar{y})^2 + \sum_{i=1}^n (y_i - \hat{y}_i)^2$$

$$\text{STCC} = \text{SCR} + \text{SCE}$$

La siguiente tabla presenta la estructura estándar del ANOVA para probar la significancia global de la regresión ($H_0: \beta_1 = \beta_2 = \dots = \beta_k = 0$ frente a $H_1$: al menos un $\beta_j \neq 0$).

| Fuente de Variación | Suma de Cuadrados (SS) | Grados de Libertad (gl) | Cuadrado Medio (MS) | Estadístico de Prueba $F$ |
| :--- | :--- | :--- | :--- | :--- |
| **Regresión** | $\text{SCR} = \mathbf{b}'\mathbf{X}'\mathbf{y} - \frac{(\sum y_i)^2}{n}$ | $k$ | $\text{CMR} = \frac{\text{SCR}}{k}$ | $f_0 = \frac{\text{CMR}}{\text{CME}}$ |
| **Error (Residual)** | $\text{SCE} = \mathbf{y}'\mathbf{y} - \mathbf{b}'\mathbf{X}'\mathbf{y}$ | $n - k - 1$ | $s^2 = \text{CME} = \frac{\text{SCE}}{n - k - 1}$ | |
| **Total Corregido** | $\text{STCC} = S_{yy} = \sum y_i^2 - \frac{(\sum y_i)^2}{n}$ | $n - 1$ | | |

Si $f_0 > f_{\alpha}(k, n - k - 1)$, se rechaza $H_0$ a un nivel de significancia $\alpha$, concluyendo que el modelo explica una cantidad estadísticamente significativa de la variación de $Y$.

### 5.3 Coeficientes de Determinación ($R^2$ y $R^2_{\text{ajustado}}$) y Correlación ($r$)

- **Coeficiente de Determinación ($R^2$)**: Mide la proporción de la variación total de la respuesta que es explicada por el modelo de regresión:
  $$R^2 = \frac{\text{SCR}}{\text{STCC}} = 1 - \frac{\text{SCE}}{\text{STCC}}, \quad 0 \le R^2 \le 1$$

- **Coeficiente de Determinación Ajustado ($R^2_{\text{ajus}}$)**: Castiga la adición de regresores innecesarios que no reducen significativamente la suma de cuadrados del error:
  $$R^2_{\text{ajus}} = 1 - \left[\frac{\text{SCE} / (n - k - 1)}{\text{STCC} / (n - 1)}\right] = 1 - \frac{s^2}{S_{yy} / (n - 1)}$$

- **Coeficiente de Correlación de Pearson ($r$)**: Mide la fuerza y dirección de la asociación lineal bivariada:
  $$r = \frac{S_{xy}}{\sqrt{S_{xx} S_{yy}}}, \quad -1 \le r \le 1$$
  En la Regresión Lineal Simple se cumple algebraicamente que $r^2 = R^2$.

### 5.4 Intervalos de Confianza y de Predicción
Para evaluar estimaciones puntuales en un valor dado del regresor $x_0$ (o vector $\mathbf{x}_0 = [1, x_{10}, x_{20}, \dots, x_{k0}]'$):

1. **Intervalo de Confianza del $(1-\alpha)100\%$ para la Respuesta Media $\mu_{Y|x_0}$**:
   - En RLS:
     $$\hat{y}_0 \pm t_{\alpha/2, n-2} \, s \sqrt{\frac{1}{n} + \frac{(x_0 - \bar{x})^2}{S_{xx}}}$$
   - En Regresión Múltiple (Matricial):
     $$\hat{y}_0 \pm t_{\alpha/2, n-k-1} \, s \sqrt{\mathbf{x}_0' (\mathbf{X}'\mathbf{X})^{-1} \mathbf{x}_0}$$

2. **Intervalo de Predicción del $(1-\alpha)100\%$ para una Observación Futura $y_0$**:
   - En RLS:
     $$\hat{y}_0 \pm t_{\alpha/2, n-2} \, s \sqrt{1 + \frac{1}{n} + \frac{(x_0 - \bar{x})^2}{S_{xx}}}$$
   - En Regresión Múltiple (Matricial):
     $$\hat{y}_0 \pm t_{\alpha/2, n-k-1} \, s \sqrt{1 + \mathbf{x}_0' (\mathbf{X}'\mathbf{X})^{-1} \mathbf{x}_0}$$

---

## 6. Diagnóstico Avanzado y Selección de Modelos (Walpole Cap. 12)

Walpole dedica atención especial a las herramientas de validación diagnóstica para prevenir el sobreajuste y verificar el cumplimiento de los supuestos del modelo.

### 6.1 Prueba de Falta de Ajuste (Lack-of-Fit Test)
Cuando se dispone de **observaciones repetidas** de $y$ para un mismo valor de $x$ (o combinación de regresores), la Suma de Cuadrados del Error se puede descomponer en dos componentes ortogonales:

$$\text{SCE} = \text{SCE}_{\text{error puro}} + \text{SCE}_{\text{falta de ajuste}}$$

1. **Suma de Cuadrados del Error Puro**: Mide la variabilidad aleatoria inherente del proceso entre réplicas:
   $$\text{SCE}_{\text{error puro}} = \sum_{i=1}^k \sum_{j=1}^{n_i} (y_{ij} - \bar{y}_{i\cdot})^2, \quad \text{gl} = n - k$$
   donde $k$ es el número de niveles distintos del regresor y $n = \sum n_i$.
2. **Suma de Cuadrados de Falta de Ajuste**: Mide la variación sistemática introducida por omitir términos de orden superior (p. ej. términos cuadráticos):
   $$\text{SCE}_{\text{falta de ajuste}} = \text{SCE} - \text{SCE}_{\text{error puro}}, \quad \text{gl} = k - 2$$

El estadístico de prueba para evaluar la inadecuación del modelo lineal es:

$$f_0 = \frac{\text{SCE}_{\text{falta de ajuste}} / (k - 2)}{\text{SCE}_{\text{error puro}} / (n - k)}$$

Si $f_0 > f_{\alpha}(k-2, n-k)$, se concluye que existe falta de ajuste significativa y el modelo lineal resulta insuficiente.

### 6.2 Matriz Sombrero ($\mathbf{H}$) y Apalancamiento ($h_{ii}$)
La **Matriz Sombrero** ajusta los valores observados $\mathbf{y}$ para proyectarlos en los valores predichos $\hat{\mathbf{y}} = \mathbf{H}\mathbf{y}$:

$$\mathbf{H} = \mathbf{X} (\mathbf{X}'\mathbf{X})^{-1} \mathbf{X}'$$

Los elementos de la diagonal principal $h_{ii}$ cuantifican el **apalancamiento (leverage)** del $i$-ésimo dato en el espacio de regresores:
- Se cumple que $\sum_{i=1}^n h_{ii} = k + 1$.
- Un punto se considera de alto apalancamiento si $h_{ii} > \frac{2(k+1)}{n}$.
- La varianza del residual $e_i$ depende directamente de $h_{ii}$: $\text{Var}(e_i) = \sigma^2 (1 - h_{ii})$.

### 6.3 Estadístico $C_p$ de Mallows y Residuales PRESS
- **Estadístico $C_p$ de Mallows**: Evalúa subconjuntos de modelos candidatos balanceando el sesgo por subajuste frente a la varianza por sobreajuste:
  $$C_p = p + \frac{(s^2 - \hat{\sigma}^2)(n - p)}{\hat{\sigma}^2}$$
  donde $p = k_p + 1$ es el número de parámetros del modelo candidato, $s^2$ es su error cuadrático medio y $\hat{\sigma}^2$ es la estimación del error del modelo completo. Un modelo adecuado presenta un $C_p \approx p$.

- **Residuales PRESS y Validación Cruzada**: Miden el error de predicción fuera de la muestra mediante la técnica de "eliminar uno" (leave-one-out):
  $$e_{i, -i} = \frac{e_i}{1 - h_{ii}}$$
  $$\text{PRESS} = \sum_{i=1}^n e_{i, -i}^2 = \sum_{i=1}^n \left(\frac{e_i}{1 - h_{ii}}\right)^2$$
  El modelo con el valor de $\text{PRESS}$ más bajo exhibe el mayor poder predictivo en datos independientes.

### 6.4 Algoritmos Secuenciales de Selección de Regresores
Walpole describe tres procedimientos automatizados para seleccionar el mejor subconjunto de variables:

1. **Selección hacia Adelante (Forward Selection)**: Inicia sin variables en el modelo e ingresa en cada paso la variable con mayor aporte a la $\text{SCR}$ (mayor valor de $F$ parcial) si supera el umbral $\alpha_{\text{entrada}}$.
2. **Eliminación hacia Atrás (Backward Elimination)**: Inicia con el modelo completo con todos los regresores y elimina secuencialmente la variable con menor significancia (menor valor de $F$ parcial) si su $P$-valor supera el umbral $\alpha_{\text{salida}}$.
3. **Regresión por Etapas (Stepwise Regression)**: Modificación de la selección hacia adelante que reevalúa en cada paso todas las variables previamente ingresadas para eliminar aquellas que hayan perdido significancia tras la incorporación de nuevos regresores.

# Teoría de Ajuste por Mínimos Cuadrados y Modelos de Regresión

El análisis numérico y el modelado estadístico en ingeniería requieren fundamentar las relaciones entre variables observadas mediante modelos matemáticos rigurosos. La caracterización de fenómenos experimentales mediante funciones empíricas permite estimar el comportamiento de un sistema a partir de datos muestrales, aplicando criterios de optimización para garantizar la representatividad del ajuste.

---

## 1. Fundamentos Estadísticos de la Regresión y la Correlación

En el estudio de fenómenos físicos e ingenieriles, resulta indispensable analizar si existe una relación funcional entre dos o más variables dependientes e independientes. Tal como se documenta en las obras de referencia sobre métodos numéricos aplicados de Daniel Francisco Campos Aranda y en los materiales teóricos de la cátedra de Análisis Numérico de la UTN FRLP, estas relaciones deben respaldarse en principios causa-efecto que expliquen el fenómeno bajo estudio.

### 1.1. Regresión frente a Correlación
* **Regresión**: Es el procedimiento analítico mediante el cual se obtiene una ecuación matemática que vincula una variable dependiente $y$ con una o más variables independientes $x_1, x_2, \dots, x_k$. La función obtenida permite predecir o estimar valores de $y$ (denotado formalmente como $y_{\text{ajuste}}$ o $y_{\text{pronóstico}}$) a partir de valores conocidos de $x$.
* **Correlación**: Cuantifica la intensidad o el grado de asociación o dependencia entre la variable dependiente y las variables independientes. Esta medida se expresa numéricamente a través del coeficiente de correlación.

### 1.2. Diagramas de Dispersión y Selección de Modelos
Un diagrama de dispersión consiste en la representación gráfica en el plano cartesiano $\mathbb{R}^2$ del conjunto de pares ordenados experimentales observados $\{(x_i, y_i)\}_{i=1}^n$. El análisis visual de la nube de puntos permite identificar intuitivamente la tendencia del fenómeno —distinguiendo entre patrones de regresión lineal y no lineal— y detectar valores atípicos que por lo general se deben a errores en la muestra o en las mediciones.

---

## 2. Principio y Deducción Matemática de Mínimos Cuadrados

El método de mínimos cuadrados busca determinar la función empírica que mejor representa a una nube de puntos experimentales.

### 2.1. Criterio de Optimización y Función de Desviación
Dado un conjunto de datos experimentales expresado mediante la tabla:

$$\{(x_1, y_1), (x_2, y_2), \dots, (x_n, y_n)\}$$

y suponiendo conocida la forma de una función empírica propuesta:

$$y = f(x, a_1, a_2, \dots, a_m)$$

donde $a_1, a_2, \dots, a_m$ son los coeficientes o parámetros a determinar, se define la desviación o error $\varepsilon_i$ para la $i$-ésima observación como la diferencia entre la ordenada de la fórmula empírica y el valor experimental observado:

$$\varepsilon_i = f(x_i, a_1, a_2, \dots, a_m) - y_i$$

El principio de mínimos cuadrados establece que, de todas las rectas o curvas que representan a una nube de puntos, la de mejor ajuste es aquella que minimiza la suma de los cuadrados de las distancias o desviaciones de cada punto a dicha curva. Se utiliza el cuadrado de las desviaciones para evitar la cancelación algebraica entre errores positivos y negativos, garantizando que el signo de la discrepancia no altere la optimización.

### 2.2. Condición de Extremo Relativo
Se define la función de error acumulado $\delta(a_1, a_2, \dots, a_m)$ como:

$$\delta(a_1, a_2, \dots, a_m) = \sum_{i=1}^{n} \left[ f(x_i, a_1, a_2, \dots, a_m) - y_i \right]^2$$

Para determinar los coeficientes que hacen mínima a la función $\delta$, se aplican las condiciones necesarias para un valor extremo en funciones de varias variables, igualando a cero las derivadas parciales respecto a cada coeficiente:

$$\frac{\partial \delta}{\partial a_1} = 0, \quad \frac{\partial \delta}{\partial a_2} = 0, \quad \dots, \quad \frac{\partial \delta}{\partial a_m} = 0$$

Si este sistema de ecuaciones simultáneas tiene solución y es única, dicha solución corresponde al conjunto de parámetros de ajuste buscado.

---

## 3. Formulación Matricial del Sistema de Ecuaciones Normales

Cuando la función empírica propuesta es lineal respecto a los coeficientes $a_1, a_2, \dots, a_m$, se puede expresar como una combinación lineal de funciones base $\{\phi_j(x)\}_{j=1}^m$:

$$f(x, a_1, a_2, \dots, a_m) = a_1 \phi_1(x) + a_2 \phi_2(x) + \dots + a_m \phi_m(x)$$

Al sustituir esta representación lineal en la función del cuadrado de las desviaciones $\delta$, se obtiene:

$$\delta(a_1, a_2, \dots, a_m) = \sum_{i=1}^{n} \left[ a_1 \phi_1(x_i) + a_2 \phi_2(x_i) + \dots + a_m \phi_m(x_i) - y_i \right]^2$$

Diferenciando parcialmente la función $\delta$ respecto a cada uno de los coeficientes $a_k$ (con $k = 1, 2, \dots, m$) e igualando a cero:

$$\frac{\partial \delta}{\partial a_k} = 2 \sum_{i=1}^{n} \left[ a_1 \phi_1(x_i) + a_2 \phi_2(x_i) + \dots + a_m \phi_m(x_i) - y_i \right] \phi_k(x_i) = 0$$

Dividiendo entre 2 y distribuyendo las sumatorias, se obtiene la $k$-ésima ecuación del sistema:

$$a_1 \sum_{i=1}^{n} \phi_1(x_i) \phi_k(x_i) + a_2 \sum_{i=1}^{n} \phi_2(x_i) \phi_k(x_i) + \dots + a_m \sum_{i=1}^{n} \phi_m(x_i) \phi_k(x_i) = \sum_{i=1}^{n} y_i \phi_k(x_i)$$

Al expandir estas ecuaciones para $k = 1, 2, \dots, m$, se forma el sistema general de ecuaciones normales, el cual adopta la siguiente representación matricial explícita:

$$\begin{bmatrix} \sum \phi_1(x_i)\phi_1(x_i) & \sum \phi_2(x_i)\phi_1(x_i) & \dots & \sum \phi_m(x_i)\phi_1(x_i) \\ \sum \phi_1(x_i)\phi_2(x_i) & \sum \phi_2(x_i)\phi_2(x_i) & \dots & \sum \phi_m(x_i)\phi_2(x_i) \\ \vdots & \vdots & \ddots & \vdots \\ \sum \phi_1(x_i)\phi_m(x_i) & \sum \phi_2(x_i)\phi_m(x_i) & \dots & \sum \phi_m(x_i)\phi_m(x_i) \end{bmatrix} \begin{bmatrix} a_1 \\ a_2 \\ \vdots \\ a_m \end{bmatrix} = \begin{bmatrix} \sum y_i \phi_1(x_i) \\ \sum y_i \phi_2(x_i) \\ \vdots \\ \sum y_i \phi_m(x_i) \end{bmatrix}$$

---

## 4. Desarrollo Riguroso de Casos Particulares

A partir de la formulación general de las ecuaciones normales, se deducen los sistemas matriciales específicos para las formas funcionales más utilizadas en la práctica.

### 4.1. Ajuste Lineal
Para la ecuación de una recta dada por $y = a_1 + a_2 x = y_{\text{ajuste}} = y_{\text{pronóstico}}$, las funciones base son $\phi_1(x) = 1$ y $\phi_2(x) = x$. 

Sustituyendo estas funciones base en el sistema general de ecuaciones normales, se llega al sistema:

$$a_1 \sum_{i=1}^{n} 1 + a_2 \sum_{i=1}^{n} x_i = \sum_{i=1}^{n} y_i$$

$$a_1 \sum_{i=1}^{n} x_i + a_2 \sum_{i=1}^{n} x_i^2 = \sum_{i=1}^{n} y_i x_i$$

expresado matricialmente como:

$$\begin{bmatrix} n & \sum_{i=1}^{n} x_i \\ \sum_{i=1}^{n} x_i & \sum_{i=1}^{n} x_i^2 \end{bmatrix} \begin{bmatrix} a_1 \\ a_2 \end{bmatrix} = \begin{bmatrix} \sum_{i=1}^{n} y_i \\ \sum_{i=1}^{n} y_i x_i \end{bmatrix}$$

Este sistema constituye la referencia fundamental para los modelos linealizados.

### 4.2. Ajuste Polinómico de Segundo Orden y Grado Genérico $k$

#### Ajuste Polinómico de Segundo Orden (Parabólico)
La función empírica es $y = a_1 + a_2 x + a_3 x^2 = y_{\text{ajuste}}$, por lo que las funciones base corresponden a $\phi_1(x) = 1$, $\phi_2(x) = x$ y $\phi_3(x) = x^2$.

El desarrollo de las ecuaciones normales conduce al sistema matricial $3 \times 3$:

$$\begin{bmatrix} n & \sum_{i=1}^{n} x_i & \sum_{i=1}^{n} x_i^2 \\ \sum_{i=1}^{n} x_i & \sum_{i=1}^{n} x_i^2 & \sum_{i=1}^{n} x_i^3 \\ \sum_{i=1}^{n} x_i^2 & \sum_{i=1}^{n} x_i^3 & \sum_{i=1}^{n} x_i^4 \end{bmatrix} \begin{bmatrix} a_1 \\ a_2 \\ a_3 \end{bmatrix} = \begin{bmatrix} \sum_{i=1}^{n} y_i \\ \sum_{i=1}^{n} y_i x_i \\ \sum_{i=1}^{n} y_i x_i^2 \end{bmatrix}$$

#### Ajuste Polinómico General de Grado $k$
Para una función de grado $k$ expresada como $y = a_1 + a_2 x + a_3 x^2 + \dots + a_k x^{k-1} + a_{k+1} x^k = y_{\text{ajuste}}$, las funciones base son $\phi_1(x) = 1, \phi_2(x) = x, \dots, \phi_{k+1}(x) = x^k$.

El sistema matricial resultante de dimensión $(k+1) \times (k+1)$ toma la siguiente forma:

$$\begin{bmatrix} n & \sum x_i & \dots & \sum x_i^{k-1} & \sum x_i^k \\ \sum x_i & \sum x_i^2 & \dots & \sum x_i^k & \sum x_i^{k+1} \\ \vdots & \vdots & \ddots & \vdots & \vdots \\ \sum x_i^k & \sum x_i^{k+1} & \dots & \sum x_i^{2k-1} & \sum x_i^{2k} \end{bmatrix} \begin{bmatrix} a_1 \\ a_2 \\ \vdots \\ a_{k+1} \end{bmatrix} = \begin{bmatrix} \sum y_i \\ \sum y_i x_i \\ \vdots \\ \sum y_i x_i^k \end{bmatrix}$$

### 4.3. Modelos No Lineales y Métodos de Linealización
Cuando la función empírica no es lineal con respecto a sus coeficientes, es indispensable realizar una transformación algebraica que linealice la ecuación, permitiendo resolver el problema mediante el sistema matricial de primer orden en un espacio de variables transformadas.

El siguiente cuadro analítico resume las trasformaciones matemáticas requeridas para la linealización de los principales modelos no lineales:

| Modelo No Lineal | Proceso de Linealización | Ecuación Linealizada Equivalente ($Y = a_1 + a_2 X$) | Transformaciones de Variables y Coeficientes |
| :--- | :--- | :--- | :--- |
| **Exponencial**: $y = a \cdot e^{b x}$ | Aplicación de $\ln$ a ambos lados: $\ln y = \ln a + b x \cdot \ln e$ | $\ln y = \ln a + b x$ | $Y = \ln y$, $a_1 = \ln a$, $a_2 = b$, $X = x$ |
| **Potencial**: $y = a \cdot x^b$ | Aplicación de $\ln$ a ambos lados: $\ln y = \ln a + b \ln x$ | $\ln y = \ln a + b \ln x$ | $Y = \ln y$, $a_1 = \ln a$, $a_2 = b$, $X = \ln x$ |
| **Cociente (Crecimiento Saturado)**: $y = \frac{a x}{b + x}$ | Inversión de términos: $\frac{1}{y} = \frac{b + x}{a x} = \frac{b}{a} \cdot \frac{1}{x} + \frac{1}{a}$ | $\frac{1}{y} = \frac{1}{a} + \frac{b}{a} \left(\frac{1}{x}\right)$ | $Y = \frac{1}{y}$, $a_1 = \frac{1}{a}$, $a_2 = \frac{b}{a}$, $X = \frac{1}{x}$ |

#### Planteo Matricial del Ajuste Exponencial
Comparando la ecuación linealizada $\ln y = \ln a + b x$ con el modelo lineal de referencia, el sistema matricial para resolver los parámetros transformados $\ln a$ y $b$ es:

$$\begin{bmatrix} n & \sum_{i=1}^{n} x_i \\ \sum_{i=1}^{n} x_i & \sum_{i=1}^{n} x_i^2 \end{bmatrix} \begin{bmatrix} \ln a \\ b \end{bmatrix} = \begin{bmatrix} \sum_{i=1}^{n} \ln y_i \\ \sum_{i=1}^{n} x_i \cdot \ln y_i \end{bmatrix}$$

Una vez obtenido $\ln a$, el valor del coeficiente $a$ original se calcula mediante la función exponencial $a = e^{\ln a}$.

#### Planteo Matricial del Ajuste Potencial
Asociando $\ln y = \ln a + b \ln x$ a la forma lineal, las ecuaciones normales adoptan la estructura:

$$\begin{bmatrix} n & \sum_{i=1}^{n} \ln x_i \\ \sum_{i=1}^{n} \ln x_i & \sum_{i=1}^{n} (\ln x_i)^2 \end{bmatrix} \begin{bmatrix} \ln a \\ b \end{bmatrix} = \begin{bmatrix} \sum_{i=1}^{n} \ln y_i \\ \sum_{i=1}^{n} \ln x_i \cdot \ln y_i \end{bmatrix}$$

#### Planteo Matricial del Ajuste del Cociente
Con las variables transformadas $Y = \frac{1}{y}$ y $X = \frac{1}{x}$, el sistema matricial de ajuste es:

$$\begin{bmatrix} n & \sum_{i=1}^{n} \frac{1}{x_i} \\ \sum_{i=1}^{n} \left(\frac{1}{x_i}\right) & \sum_{i=1}^{n} \left(\frac{1}{x_i}\right)^2 \end{bmatrix} \begin{bmatrix} \frac{1}{a} \\ \frac{b}{a} \end{bmatrix} = \begin{bmatrix} \sum_{i=1}^{n} \frac{1}{y_i} \\ \sum_{i=1}^{n} \left(\frac{1}{x_i} \cdot \frac{1}{y_i}\right) \end{bmatrix}$$

Resuelto el sistema para $a_1 = \frac{1}{a}$ y $a_2 = \frac{b}{a}$, los coeficientes empíricos originales se recuperan como $a = \frac{1}{a_1}$ y $b = a_2 \cdot a$.

---

## 5. Cuantificación de la Bondad del Ajuste y Validación

La determinación de una ecuación de ajuste requiere estimar cuantitativamente si el modelo obtenido representa con fidelidad la distribución de los datos experimentales.

### 5.1. Coeficiente de Determinación ($r^2$)
La bondad del ajuste es un parámetro que cuantifica la proporción de la varianza observada en los datos que es explicada por la función de regresión. Se define mediante la expresión:

$$r^2 = \frac{S_T - S_R}{S_T}$$

donde:
* $S_T$: Suma total de los cuadrados de las desviaciones alrededor de la media o valor promedio.
* $S_R$: Suma total de los cuadrados de los residuos de regresión.

El coeficiente de determinación $r^2$ toma siempre un valor numérico comprendido en el intervalo $[0, 1]$. En la práctica de ingeniería y ciencias aplicadas, un valor de $r^2 > 0.85$ se considera indicador de un buen ajuste que representa satisfactoriamente el comportamiento de los datos analizados en el corto plazo.

### 5.2. Ecuaciones Específicas de $S_T$ y $S_R$ según el Espacio del Modelo

#### 1. Caso Lineal y Polinómicos (Todos los grados)
El cálculo se realiza directamente sobre los valores observados $y_i$:

$$\bar{y} = \frac{\sum_{i=1}^{n} y_i}{n}$$

$$S_T = \sum_{i=1}^{n} \left( y_i - \bar{y} \right)^2, \quad S_R = \sum_{i=1}^{n} \left( y_i - y_{\text{ajuste}} \right)^2$$

#### 2. Casos Exponencial y Potencial
Al haberse realizado la regresión en el espacio logarítmico, las sumatorias se evalúan sobre los valores transformados $\ln y_i$:

$$\bar{y}_{\text{media}} = \frac{\sum_{i=1}^{n} \ln y_i}{n}$$

$$S_T = \sum_{i=1}^{n} \left( \ln y_i - \bar{y}_{\text{media}} \right)^2, \quad S_R = \sum_{i=1}^{n} \left( \ln y_i - y_{\text{ajuste}} \right)^2$$

donde $y_{\text{ajuste}} = \ln a + b x_i$ para el modelo exponencial, e $y_{\text{ajuste}} = \ln a + b \ln x_i$ para el modelo potencial.

#### 3. Caso del Cociente (Crecimiento Saturado)
Las sumatorias se evalúan sobre los recíprocos de las observadas $1/y_i$:

$$\bar{y}_{\text{media}} = \frac{\sum_{i=1}^{n} \left(\frac{1}{y_i}\right)}{n}$$

$$S_T = \sum_{i=1}^{n} \left( \frac{1}{y_i} - \bar{y}_{\text{media}} \right)^2, \quad S_R = \sum_{i=1}^{n} \left( \frac{1}{y_i} - y_{\text{ajuste}} \right)^2$$

donde $y_{\text{ajuste}} = \frac{1}{a} + \frac{b}{a} \left(\frac{1}{x_i}\right)$.

### 5.3. Consideraciones Metodológicas y Capacidad Predictiva
En la evaluación de modelos de regresión para trabajos de investigación o aplicaciones profesionales de ingeniería, debe considerarse una premisa central: en el análisis de los datos, un coeficiente de determinación elevado no garantiza por sí solo una buena capacidad predictiva; también deben considerarse el comportamiento del modelo fuera del intervalo observado y la plausibilidad del fenómeno representado.

Por lo tanto, la validación del modelo debe integrar la bondad del ajuste estadístico con el análisis cualitativo y teórico de las condiciones límite y la coherencia física de los pronósticos al extrapolar valores fuera del rango experimental.

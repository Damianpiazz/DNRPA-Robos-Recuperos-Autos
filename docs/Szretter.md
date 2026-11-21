# Resumen Técnico e Inferencia Estadística en Regresión Lineal
*Basado en las lecciones y desarrollos matemáticos del "Apunte de Regresión Lineal" (María Eugenia Szretter Noste, Facultad de Ciencias Exactas y Naturales - UBA)*

---

El estudio cuantitativo de la relación entre variables continuas constituye uno de los pilares del modelado estadístico aplicado. En el apunte de regresión lineal elaborado por María Eugenia Szretter Noste para la Carrera de Especialización en Estadística para Ciencias de la Salud de la Universidad de Buenos Aires, se expone de manera rigurosa la teoría probabilística y matricial que sustenta tanto la correlación bivariada como la regresión lineal simple y múltiple. A continuación se presenta una síntesis analítica y formal de los conceptos, ecuaciones normales, propiedades muestrales, técnicas de diagnóstico y algoritmos de selección de modelos desarrollados en dicha obra.

---

## 1. Análisis de Correlación y Medidas de Asociación Bivariada

El análisis de correlación busca describir numéricamente la fuerza y dirección de la asociación empírica entre dos variables aleatorias continuas $X$ e $Y$ sin asumir formalmente una estructura de dependencia causa-efecto.

### 1.1. Diagramas de Dispersión y Desventajas de la Inspección Visual
El primer paso en el análisis exploratorio bivariado es la gráfica de dispersión (*scatter plot*). Aunque permite detectar tendencias generales, curvaturas o la presencia de valores atípicos (*outliers*), presenta limitaciones sustanciales:
* **Dependencia de la escala**: Cambios en las unidades de medida de los ejes modifican visualmente la pendiente percibida y la dispersión aparente.
* **Subjetividad de percepción**: La densidad de puntos y el rango de la ventana gráfica pueden enmascarar o exagerar asociaciones.

### 1.2. Coeficiente de Correlación de Pearson ($r$)
Para superar la subjetividad gráfica, el coeficiente de correlación producto-momento de Pearson estandariza la covarianza muestral dividiéndola por el producto de las desviaciones estándar marginales.

Dada una muestra bivariada observada $\{(X_1, Y_1), (X_2, Y_2), \dots, (X_n, Y_n)\}$, se definen las sumas de cuadrados y productos cruzados corregidos por la media:
$$S_{XX} = \sum_{i=1}^n (X_i - \bar{X})^2$$
$$S_{YY} = \sum_{i=1}^n (Y_i - \bar{Y})^2$$
$$S_{XY} = \sum_{i=1}^n (X_i - \bar{X})(Y_i - \bar{Y})$$

El coeficiente de correlación muestral de Pearson $r$ se define formalmente como:
$$r = \frac{\sum_{i=1}^n (X_i - \bar{X})(Y_i - \bar{Y})}{\sqrt{\sum_{i=1}^n (X_i - \bar{X})^2 \sum_{i=1}^n (Y_i - \bar{Y})^2}} = \frac{S_{XY}}{\sqrt{S_{XX} S_{YY}}}$$

#### Propiedades del Coeficiente de Pearson
1. **Acotamiento estricto**: $-1 \le r \le 1$. La demostración se deriva directamente de la desigualdad de Cauchy-Schwarz aplicada al producto interno de los vectores centrados.
2. **Invariancia a transformaciones lineales**: Si $U = a X + b$ y $V = c Y + d$ con $a, c \neq 0$, entonces $r_{UV} = \text{signo}(a \cdot c) \cdot r_{XY}$.
3. **Interpretación del signo y magnitud**: El signo de $r$ coincide con el de la covarianza $S_{XY}$. Un valor $|r| = 1$ indica una relación lineal determinista perfecta, mientras que $r = 0$ denota la ausencia de asociación *lineal* (lo cual no descarta relaciones funcionales no lineales).

### 1.3. Coeficiente de Correlación de Spearman ($r_s$)
Cuando el supuesto de linealidad o de normalidad bivariada no se satisface, o cuando se analizan variables medidas en escala ordinal, se utiliza el coeficiente de correlación por rangos de Spearman ($r_s$). Consiste en aplicar la fórmula de Pearson a los rangos asignados $R(X_i)$ y $R(Y_i)$ de cada observación. Evalúa si la relación funcional entre ambas variables es estrictamente monótona (creciente o decreciente), independientemente de que sea lineal.

---

## 2. El Modelo de Regresión Lineal Simple (RLS) y Estimación MCO

A diferencia de la correlación, el modelo de regresión distingue entre una **variable de respuesta o dependiente** ($Y$) y una **variable predictora, covariable o independiente** ($X$), modelando el comportamiento condicional de $Y$ dado $X$.

### 2.1. Formulación del Modelo y Supuestos Estadísticos
El modelo de regresión lineal simple postula que para cada observación $i$-ésima ($i = 1, \dots, n$):
$$Y_i = \beta_0 + \beta_1 X_i + \varepsilon_i$$

donde:
* $\beta_0$ es el parámetro escalar de la ordenada al origen (intercepto).
* $\beta_1$ es el parámetro escalar de la pendiente, que representa el cambio esperado en la respuesta $Y$ ante el incremento de una unidad en la covariable $X$.
* $\varepsilon_i$ representa la perturbación o error aleatorio inobservable.

#### Supuestos Fundamentales del Modelo Clásico
1. **Linealidad en la esperanza condicional**: $E(\varepsilon_i | X_i) = 0 \implies E(Y_i | X_i) = \beta_0 + \beta_1 X_i$. La respuesta esperada cae exactamente sobre la "verdadera" recta de regresión poblacional.
2. **Homocedasticidad (Varianza constante)**: $Var(\varepsilon_i | X_i) = \sigma^2$ para todo $i = 1, \dots, n$. La dispersión del error no depende del nivel de la covariable $X$.
3. **Independencia de los errores**: $Cov(\varepsilon_i, \varepsilon_j) = 0$ para todo $i \neq j$.
4. **Normalidad (para inferencia exacta)**: $\varepsilon_i \overset{\text{i.i.d.}}{\sim} N(0, \sigma^2)$, lo cual implica que $Y_i \overset{\text{indep.}}{\sim} N(\beta_0 + \beta_1 X_i, \sigma^2)$.

### 2.2. Deducción de los Estimadores de Mínimos Cuadrados Ordinarios (MCO)
El criterio de Mínimos Cuadrados Ordinarios busca los estimadores $\hat{\beta}_0$ y $\hat{\beta}_1$ que minimizan la función de desajuste definida por la suma de los cuadrados de las desviaciones verticales:
$$g(a, b) = \sum_{i=1}^n \left[ Y_i - (a + b X_i) \right]^2$$

Para encontrar los puntos críticos que minimizan $g(a, b)$, se igualan a cero sus derivadas parciales respecto de los parámetros $a$ y $b$:
$$\frac{\partial g}{\partial a} = -2 \sum_{i=1}^n (Y_i - a - b X_i) = 0$$
$$\frac{\partial g}{\partial b} = -2 \sum_{i=1}^n (Y_i - a - b X_i) X_i = 0$$

Al simplificar estas expresiones se obtiene el **Sistema de Ecuaciones Normales**:
$$\sum_{i=1}^n Y_i - n \hat{\beta}_0 - \hat{\beta}_1 \sum_{i=1}^n X_i = 0 \implies \sum_{i=1}^n e_i = 0$$
$$\sum_{i=1}^n Y_i X_i - \hat{\beta}_0 \sum_{i=1}^n X_i - \hat{\beta}_1 \sum_{i=1}^n X_i^2 = 0 \implies \sum_{i=1}^n e_i X_i = 0$$

Despejando analíticamente del sistema de ecuaciones normales se obtienen las expresiones estándar para los estimadores MCO:
$$\hat{\beta}_1 = \frac{\sum_{i=1}^n (X_i - \bar{X})(Y_i - \bar{Y})}{\sum_{i=1}^n (X_i - \bar{X})^2} = \frac{S_{XY}}{S_{XX}}$$
$$\hat{\beta}_0 = \bar{Y} - \hat{\beta}_1 \bar{X}$$

### 2.3. Valores Ajustados y Residuos Muestrales
A partir de los estimadores MCO, se define la **recta ajustada**:
$$\hat{Y}_i = \hat{\beta}_0 + \hat{\beta}_1 X_i$$

El **residuo muestral** $e_i$ se define como la diferencia entre el valor real observado $Y_i$ y el valor estimado por la recta $\hat{Y}_i$:
$$e_i = Y_i - \hat{Y}_i = Y_i - (\hat{\beta}_0 + \hat{\beta}_1 X_i)$$

#### Propiedades Fundamentales de los Residuos MCO
* **Suma nula**: $\sum_{i=1}^n e_i = 0 \implies \bar{e} = 0$.
* **Ortogonalidad con la covariable**: $\sum_{i=1}^n e_i X_i = 0$, lo que implica que el coeficiente de correlación muestral entre la covariable $X$ y los residuos $e$ es exactamente cero: $r(X, e) = 0$.
* **Paso por el centroide**: La recta ajustada siempre pasa por el punto medio muestral $(\bar{X}, \bar{Y})$.

---

## 3. Inferencia Estadística, Intervalos de Confianza y Análisis de Varianza (ANOVA)

### 3.1. Estimación de la Varianza del Error ($\sigma^2$)
Dado que los errores poblacionales $\varepsilon_i$ son inobservables, la varianza del error $\sigma^2$ se estima a través de la Suma de Cuadrados de los Residuos ($SSRes$):
$$SSRes = \sum_{i=1}^n e_i^2 = \sum_{i=1}^n (Y_i - \hat{Y}_i)^2$$

El estimador insesgado de $\sigma^2$, denominado **Cuadrado Medio del Error ($MSRes$)**, se obtiene dividiendo $SSRes$ por sus grados de libertad ($n - 2$, al haberse estimado 2 parámetros $\beta_0$ y $\beta_1$):
$$\hat{\sigma}^2 = MSRes = \frac{SSRes}{n - 2} = \frac{\sum_{i=1}^n (Y_i - \hat{Y}_i)^2}{n - 2}$$

La raíz cuadrada de este estimador, $\hat{\sigma} = \sqrt{MSRes}$, representa el error estándar residual de la regresión (*Residual Standard Error*).

### 3.2. Inferencia sobre la Pendiente ($\beta_1$)
Bajo los supuestos del modelo lineal normal, el estimador MCO $\hat{\beta}_1$ es una combinación lineal de las variables normales independientes $Y_i$, por lo que se distribuye normalmente:
$$\hat{\beta}_1 \sim N\left(\beta_1, \frac{\sigma^2}{S_{XX}}\right)$$

Su varianza estimada y su error estándar se expresan como:
$$\widehat{Var}(\hat{\beta}_1) = \frac{\hat{\sigma}^2}{S_{XX}} \implies se(\hat{\beta}_1) = \frac{\hat{\sigma}}{\sqrt{S_{XX}}}$$

Para contrastar la hipótesis nula de ausencia de efecto lineal $H_0: \beta_1 = 0$ frente a la alternativa $H_1: \beta_1 \neq 0$, se utiliza el estadístico de prueba $t$:
$$t = \frac{\hat{\beta}_1 - 0}{se(\hat{\beta}_1)} = \frac{\hat{\beta}_1 \sqrt{S_{XX}}}{\hat{\sigma}} \sim t_{n-2}$$

El intervalo de confianza de nivel $1 - \alpha$ para el parámetro $\beta_1$ se construye mediante:
$$\hat{\beta}_1 \pm t_{n-2, 1-\alpha/2} \cdot se(\hat{\beta}_1)$$

### 3.3. Intervalo de Confianza para la Respuesta Media e Intervalo de Predicción
Para evaluar el comportamiento predicho en un valor específico de la covariable $X = x_h$:

#### Respuesta Media Condicional $E(Y | X = x_h)$
El estimador puntual es $\hat{Y}_h = \hat{\beta}_0 + \hat{\beta}_1 x_h$. Su varianza exacta es:
$$Var(\hat{Y}_h) = \sigma^2 \left[ \frac{1}{n} + \frac{(x_h - \bar{X})^2}{S_{XX}} \right]$$

El intervalo de confianza al $1 - \alpha$ para la respuesta media $E(Y | X = x_h)$ es:
$$\hat{Y}_h \pm t_{n-2, 1-\alpha/2} \cdot \hat{\sigma} \sqrt{\frac{1}{n} + \frac{(x_h - \bar{X})^2}{S_{XX}}}$$

*Nota geométrica*: La amplitud de la banda de confianza es mínima en $x_h = \bar{X}$ y se ensancha hiperbólicamente a medida que $x_h$ se aleja de la media muestral.

#### Predicción de una Nueva Observación Individual $Y_{h(\text{nueva})}$
Al predecir el valor de un nuevo individuo independiente, se suma la variabilidad del proceso de estimación de la recta más la variabilidad inherente del nuevo error aleatorio ($\sigma^2$). La varianza del error de predicción es:
$$Var(Y_{h(\text{nueva})} - \hat{Y}_h) = \sigma^2 \left[ 1 + \frac{1}{n} + \frac{(x_h - \bar{X})^2}{S_{XX}} \right]$$

El intervalo de predicción al $1 - \alpha$ resulta:
$$\hat{Y}_h \pm t_{n-2, 1-\alpha/2} \cdot \hat{\sigma} \sqrt{1 + \frac{1}{n} + \frac{(x_h - \bar{X})^2}{S_{XX}}}$$

### 3.4. Descomposición de la Suma de Cuadrados y Tabla ANOVA
La variabilidad total de la variable de respuesta alrededor de su media se descompone algebraicamente en la suma de cuadrados explicada por la regresión y la suma de cuadrados no explicada o residual:
$$(Y_i - \bar{Y}) = (\hat{Y}_i - \bar{Y}) + (Y_i - \hat{Y}_i)$$

Al elevar al cuadrado y sumar sobre todas las observaciones (aprovechando que el término cruzado es nulo por las ecuaciones normales), se establece la **Identidad Fundamental del ANOVA**:
$$\sum_{i=1}^n (Y_i - \bar{Y})^2 = \sum_{i=1}^n (\hat{Y}_i - \bar{Y})^2 + \sum_{i=1}^n (Y_i - \hat{Y}_i)^2 \implies SSTo = SSReg + SSRes$$

#### Estructura de la Tabla de Análisis de Varianza (ANOVA)

| Fuente de Variación | Suma de Cuadrados ($SS$) | Grados de Libertad ($gl$) | Cuadrado Medio ($MS$) | Estadístico $F$ |
| :--- | :--- | :--- | :--- | :--- |
| **Regresión** | $SSReg = \sum (\hat{Y}_i - \bar{Y})^2$ | $1$ | $MSReg = \frac{SSReg}{1}$ | $F = \frac{MSReg}{MSRes}$ |
| **Residuos** | $SSRes = \sum (Y_i - \hat{Y}_i)^2$ | $n - 2$ | $MSRes = \frac{SSRes}{n - 2}$ | |
| **Total** | $SSTo = \sum (Y_i - \bar{Y})^2$ | $n - 1$ | | |

En el caso de regresión lineal simple, la prueba de hipótesis del ANOVA mediante el estadístico $F$ es equivalente a la prueba $t$ sobre la pendiente, cumpliéndose que $F = t^2$.

### 3.5. Coeficiente de Determinación ($R^2$)
El coeficiente de determinación mide la proporción de la variabilidad total observada en $Y$ que es explicada por la relación lineal con la covariable $X$:
$$R^2 = \frac{SSReg}{SSTo} = 1 - \frac{SSRes}{SSTo}$$

#### Propiedades del Coeficiente $R^2$
* $0 \le R^2 \le 1$.
* En la regresión lineal simple se cumple exactamente que $R^2 = (r_{XY})^2$, es decir, el cuadrado del coeficiente de correlación de Pearson.
* No depende de las unidades de medida de las variables.
* Representa una medida de la capacidad predictiva lineal del modelo, pero no constituye por sí solo una prueba de validez de los supuestos probabilísticos.

---

## 4. Diagnóstico de Supuestos, Residuales y Estimación Robusta

La validez de las inferencias estadísticas derivadas del modelo ajustado depende del cumplimiento estricto de los supuestos sobre las perturbaciones. El análisis diagnóstico se efectúa analizando los residuos.

### 4.1. Tipos de Residuos
* **Residuos Estandarizados**:
  $$rest_i = \frac{e_i}{\hat{\sigma}}$$
  Tienen media 0 y varianza aproximadamente igual a 1.
* **Residuos Estudentizados**:
  $$t_i = \frac{e_i}{\hat{\sigma}_{(i)} \sqrt{1 - h_{ii}}}$$
  donde $\hat{\sigma}_{(i)}$ es la estimación de la desviación estándar habiendo excluido la $i$-ésima observación, y $h_{ii}$ es el apalancamiento (*leverage*). Siguen una distribución $t$ de Student con $n - 3$ grados de libertad.

### 4.2. Patrones en Gráficos de Residuos y Remedios
Al graficar los residuos $e_i$ frente a los valores ajustados $\hat{Y}_i$ o a la covariable $X_i$:
* **Patrón en abanico o embudo**: Revela heteroscedasticidad (varianza no constante).
  * *Remedios*: Transformaciones estabilizadoras de varianza en $Y$ ($\ln Y$, $\sqrt{Y}$) o aplicación de **Mínimos Cuadrados Ponderados (WLS)**, donde se minimiza $g_{wls}(a,b) = \sum_{i=1}^n w_i (Y_i - a - bX_i)^2$ asignando pesos $w_i = 1/\sigma_i^2$.
* **Patrón curvilíneo o parabólico**: Indica falta de linealidad en la respuesta esperada.
  * *Remedios*: Transformación no lineal de $X$ o inclusión de términos polinomiales ($X^2$).

### 4.3. Observaciones Atípicas e Influyentes
Los estimadores MCO son altamente sensibles a observaciones alejadas del grueso de los datos. Se distinguen tres conceptos:
1. **Outlier en la Respuesta**: Punto con un residuo vertical excepcionalmente grande.
2. **Apalancamiento (*Leverage* $h_{ii}$)**: Mide la distancia horizontal del valor $X_i$ respecto a la media $\bar{X}$. Proviene de la diagonal principal de la matriz sombrero. Un valor $h_{ii} > \frac{2p}{n}$ indica alto apalancamiento.
3. **Punto Influyente**: Observación que altera sustancialmente las estimaciones de los coeficientes al ser eliminada. Se evalúa mediante la **Distancia de Cook ($D_i$)**:
   $$D_i = \frac{(\hat{\boldsymbol{\beta}} - \hat{\boldsymbol{\beta}}_{(i)})^T (\mathbf{X}^T \mathbf{X}) (\hat{\boldsymbol{\beta}} - \hat{\boldsymbol{\beta}}_{(i)})}{p \cdot MSRes}$$

### 4.4. Métodos de Estimación Robusta
Para evitar que observaciones atípicas distorsionen las inferencias, el apunte de Szretter introduce alternativas robustas a los MCO:
* **Least Median of Squares (LMS)**: Minimiza la mediana de los residuos al cuadrado $\text{mediana}(e_i^2)$ en lugar de la suma.
* **M-Estimadores y MM-Estimadores**: Reemplazan la función de pérdida cuadrática por una función $\rho(e_i)$ acotada y simétrica que crece más lentamente que el cuadrado para residuos grandes, reduciendo el peso asignado a los outliers en el ajuste.

---

## 5. Regresión Lineal Múltiple (RLM): Formulación Matricial y Geometría de Proyección

Cuando la variable de respuesta $Y$ depende de $p - 1$ variables explicativas $X_1, X_2, \dots, X_{p-1}$, el modelo se extiende al ámbito multivariado.

### 5.1. El Modelo Poblacional y Notación Matricial
Para $n$ observaciones independientes, el modelo de regresión lineal múltiple se expresa como:
$$Y_i = \beta_0 + \beta_1 X_{i1} + \beta_2 X_{i2} + \dots + \beta_{p-1} X_{i,p-1} + \varepsilon_i, \quad i = 1, \dots, n$$

En notación matricial compacta:
$$\mathbf{Y} = \mathbf{X} \boldsymbol{\beta} + \boldsymbol{\varepsilon}$$

donde:
$$\mathbf{Y} = \begin{bmatrix} Y_1 \\ Y_2 \\ \vdots \\ Y_n \end{bmatrix}_{n \times 1}, \quad \mathbf{X} = \begin{bmatrix} 1 & X_{11} & X_{12} & \dots & X_{1,p-1} \\ 1 & X_{21} & X_{22} & \dots & X_{2,p-1} \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ 1 & X_{n1} & X_{n2} & \dots & X_{n,p-1} \end{bmatrix}_{n \times p}, \quad \boldsymbol{\beta} = \begin{bmatrix} \beta_0 \\ \beta_1 \\ \vdots \\ \beta_{p-1} \end{bmatrix}_{p \times 1}, \quad \boldsymbol{\varepsilon} = \begin{bmatrix} \varepsilon_1 \\ \varepsilon_2 \\ \vdots \\ \varepsilon_n \end{bmatrix}_{n \times 1}$$

Los supuestos matriciales se resumen en:
$$E(\boldsymbol{\varepsilon} | \mathbf{X}) = \mathbf{0}_{n \times 1}$$
$$Var(\boldsymbol{\varepsilon} | \mathbf{X}) = \sigma^2 \mathbf{I}_n$$
$$\boldsymbol{\varepsilon} \sim N_n(\mathbf{0}, \sigma^2 \mathbf{I}_n)$$

### 5.2. Interpretación de los Coeficientes Parciales
El coeficiente $\beta_j$ representa el **efecto parcial** de la covariable $X_j$: es el cambio esperado en la respuesta media $E(Y)$ por cada incremento unitario en $X_j$, manteniendo fijas y constantes las restantes $p - 2$ covariables del modelo (efecto aditivo sin interacción).

### 5.3. Ecuaciones Normales y Solución MCO Matricial
La minimización de la suma de cuadrados del error $g(\boldsymbol{\beta}) = \boldsymbol{\varepsilon}^T \boldsymbol{\varepsilon} = (\mathbf{Y} - \mathbf{X}\boldsymbol{\beta})^T (\mathbf{Y} - \mathbf{X}\boldsymbol{\beta})$ conduce al sistema de ecuaciones normales matriciales:
$$\mathbf{X}^T \mathbf{X} \hat{\boldsymbol{\beta}} = \mathbf{X}^T \mathbf{Y}$$

Si la matriz de diseño $\mathbf{X}$ tiene rango columna completo ($\text{rango}(\mathbf{X}) = p$), la matriz de datos $\mathbf{X}^T \mathbf{X}$ es invertible y la solución única MCO es:
$$\hat{\boldsymbol{\beta}} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{Y}$$

### 5.4. Geometría de Proyección: La Matriz Sombrero ($\mathbf{H}$)
El vector de valores ajustados $\hat{\mathbf{Y}}$ se obtiene proyectando ortogonalmente el vector de observaciones $\mathbf{Y}$ sobre el espacio columna de la matriz de diseño $C(\mathbf{X})$:
$$\hat{\mathbf{Y}} = \mathbf{X} \hat{\boldsymbol{\beta}} = \mathbf{X} (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{Y} = \mathbf{H} \mathbf{Y}$$

La matriz $\mathbf{H} = \mathbf{X} (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T$ se denomina **Matriz Sombrero (*Hat Matrix*)**.

#### Propiedades Matemáticas de la Matriz Sombrero ($\mathbf{H}$)
1. **Simetría e Idempotencia**: $\mathbf{H}^T = \mathbf{H}$ y $\mathbf{H}^2 = \mathbf{H}$.
2. **Propiedades del Vector de Residuos**:
   $$\mathbf{e} = \mathbf{Y} - \hat{\mathbf{Y}} = (\mathbf{I}_n - \mathbf{H}) \mathbf{Y}$$
   La matriz $(\mathbf{I}_n - \mathbf{H})$ proyecta sobre el complemento ortogonal de $C(\mathbf{X})$.
3. **Matriz de Varianza-Covarianza de los Residuos**:
   $$Var(\mathbf{e}) = \sigma^2 (\mathbf{I}_n - \mathbf{H})$$
   Esto demuestra formalmente que los residuos muestrales no son independientes y presentan varianzas heterogéneas $Var(e_i) = \sigma^2 (1 - h_{ii})$.
4. **Traza y Grados de Libertad**: $\text{traza}(\mathbf{H}) = p$ y $\text{traza}(\mathbf{I}_n - \mathbf{H}) = n - p$.

### 5.5. Inferencia Matricial y ANOVA Múltiple
* **Estimador de $\sigma^2$**: $\hat{\sigma}^2 = MSRes = \frac{\mathbf{e}^T \mathbf{e}}{n - p} = \frac{SSRes}{n - p}$.
* **Matriz de Varianza de los Coeficientes**: $Var(\hat{\boldsymbol{\beta}}) = \sigma^2 (\mathbf{X}^T \mathbf{X})^{-1} \implies \widehat{Var}(\hat{\boldsymbol{\beta}}) = \hat{\sigma}^2 (\mathbf{X}^T \mathbf{X})^{-1}$.
* **Test $F$ Global de Significancia de la Regresión**:
  $$H_0: \beta_1 = \beta_2 = \dots = \beta_{p-1} = 0 \quad \text{vs} \quad H_1: \exists j / \beta_j \neq 0$$
  $$F = \frac{MSReg}{MSRes} = \frac{SSReg / (p - 1)}{SSRes / (n - p)} \sim F_{p-1, n-p}$$
* **Coeficiente de Determinación Múltiple Ajustado ($R^2_{\text{ajustado}}$)**:
  $$R^2_{\text{ajustado}} = 1 - \frac{SSRes / (n - p)}{SSTo / (n - 1)} = 1 - \left( \frac{n - 1}{n - p} \right) (1 - R^2)$$
  Penaliza la inclusión artificial de covariables irrelevantes que incrementan trivialmente el $R^2$ ordinario.

---

## 6. Modelos Anidados, Variables Categóricas, Multicolinealidad y Selección de Modelos

### 6.1. Comparación de Modelos Anidados (Test $F$ Parcial)
Para evaluar si un conjunto de $p - q$ covariables aporta información predictiva estadísticamente significativa sobre un modelo reducido con $q$ parámetros:
$$\text{Modelo Reducido (Simple): } Y_i = \beta_0 + \sum_{j=1}^{q-1} \beta_j X_{ij} + \varepsilon_i$$
$$\text{Modelo Completo: } Y_i = \beta_0 + \sum_{j=1}^{q-1} \beta_j X_{ij} + \sum_{j=q}^{p-1} \beta_j X_{ij} + \varepsilon_i$$

Se contrasta la hipótesis nula $H_0: \beta_q = \beta_{q+1} = \dots = \beta_{p-1} = 0$ mediante la prueba de la razón de varianzas parciales:
$$F = \frac{(SSRes_{\text{reducido}} - SSRes_{\text{completo}}) / (p - q)}{SSRes_{\text{completo}} / (n - p)} \sim F_{p-q, n-p}$$

### 6.2. Inclusión de Variables Categóricas e Interacciones
* **Variables Indicadoras (*Dummy*)**: Una variable cualitativa con $K$ niveles se codifica mediante $K - 1$ variables dicotómicas $\{0, 1\}$ para evitar la trampa de la variable dummy (colinealidad exacta con el intercepto).
* **Interacción de Factores**: La inclusión de términos multiplicativos ($X_1 \cdot X_2$) permite que la pendiente o efecto marginal de la covariable $X_1$ dependa del nivel adoptado por $X_2$:
  $$E(Y | X_1, X_2) = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \beta_{12} X_1 X_2 = (\beta_0 + \beta_2 X_2) + (\beta_1 + \beta_{12} X_2) X_1$$

### 6.3. Diagnóstico y Tratamiento de la Multicolinealidad
La multicolinealidad ocurre cuando existe una fuerte relación lineal entre dos o más variables predictoras.
* **Consecuencias**: Inestabilidad numérica en la inversión de $\mathbf{X}^T \mathbf{X}$, inflación extrema de las varianzas de los estimadores $Var(\hat{\beta}_j)$ e imposibilidad de aislar los efectos individuales.
* **Diagnóstico**: Examen de la matriz de correlación entre covariables y cálculo de los Factores de Inflación de la Varianza ($VIF$).
* **Soluciones**: Centrado de variables ($X_j - \bar{X}_j$), eliminación secuencial de variables redundantes, regresión por componentes principales (PCR) o métodos de regularización con penalización (Ridge, LASSO).

### 6.4. Criterios y Métodos para la Selección de Modelos
Cuando se dispone de un número elevado de regresores potenciales, la selección del subconjunto óptimo se realiza comparando métricas cuantitativas:
1. **Criterio $C_p$ de Mallows**: Evalúa el error cuadrático medio total de predicción penalizando el sesgo por subajuste:
   $$C_p = \frac{SSRes_p}{\hat{\sigma}^2_{\text{completo}}} - (n - 2p)$$
   Un modelo adecuado presenta $C_p \approx p$.
2. **Métodos Secuenciales Algorítmicos**:
   * **Selección hacia Adelante (*Forward*)**: Incorpora iterativamente la covariable con mayor contribución marginal a $SSReg$.
   * **Eliminación hacia Atrás (*Backward*)**: Comienza con el modelo completo y retira secuencialmente la covariable menos significativa.
   * **Regresión por Etapas (*Stepwise*)**: Combina la inserción de variables del *forward* con la reevaluación continua de la significancia de las variables previamente introducidas para su eventual remoción.

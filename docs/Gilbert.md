# Guía Práctica y Formulario de Álgebra Lineal en Ciencia de Datos: Mínimos Cuadrados y Regresión
*Basado exclusivamente en la obra "Álgebra Lineal en Ciencia de Datos" de Gilbert Strang*

---

## 1. Geometría de los Cuatro Subespacios Fundamentales

El estudio del ajuste por mínimos cuadrados requiere comprender la estructura geométrica que imponen los cuatro subespacios fundamentales asociados a una matriz $A \in \mathbb{R}^{m \times n}$ de rango $r$, de acuerdo con el marco unificado presentado por Gilbert Strang.

### Definición y Dimensiones de los Subespacios

Para cualquier matriz rectangular $A$ con $m$ filas (observaciones) y $n$ columnas (parámetros o regresores):

1. **Espacio Columna $C(A) \subseteq \mathbb{R}^m$**: Es el subespacio generado por las columnas de $A$. Su dimensión es el rango de la matriz, $\dim(C(A)) = r$. Representa el conjunto de todos los vectores de respuesta posibles $Ax$ que el modelo lineal puede generar exactamente.
2. **Espacio Nulo $N(A) \subseteq \mathbb{R}^n$**: Es el conjunto de soluciones a la ecuación homogénea $Ax = 0$. Su dimensión es $\dim(N(A)) = n - r$.
3. **Espacio Fila $C(A^T) \subseteq \mathbb{R}^n$**: Es el subespacio generado por las filas de $A$ (o columnas de $A^T$). Su dimensión es $\dim(C(A^T)) = r$.
4. **Espacio Nulo Izquierdo $N(A^T) \subseteq \mathbb{R}^m$**: Es el conjunto de soluciones a $A^T y = 0$. Su dimensión es $\dim(N(A^T)) = m - r$.

### Teorema Fundamental del Álgebra Lineal y Ortogonalidad

Gilbert Strang establece que los cuatro subespacios se organizan en dos pares de complementos ortogonales en sus respectivos espacios euclídeos:

- En $\mathbb{R}^n$, el espacio fila y el espacio nulo son ortogonales entre sí:
  $$N(A) = C(A^T)^\perp, \quad \dim(C(A^T)) + \dim(N(A)) = r + (n - r) = n$$

- En $\mathbb{R}^m$, el espacio columna y el espacio nulo izquierdo son ortogonales entre sí:
  $$N(A^T) = C(A)^\perp, \quad \dim(C(A)) + \dim(N(A^T)) = r + (m - r) = m$$

Esta descomposición implica que todo vector de observaciones $b \in \mathbb{R}^m$ se descompone de forma única en la suma de dos componentes perpendiculares:
$$b = p + e$$

donde $p \in C(A)$ es la proyección ortogonal de $b$ sobre el espacio columna, y $e \in N(A^T)$ es el vector de error u ortogonal a $C(A)$.

---

## 2. Proyección Ortogonal y Ecuaciones Normales

Cuando un sistema de ecuaciones lineales $Ax = b$ es sobredeterminado ($m > n$) e inconsistente debido al ruido muestral u observaciones imperfectas, el vector $b$ no pertenece al espacio columna $C(A)$. En tales circunstancias, no existe ningún vector $x$ que satisfaga la igualdad exacta.

### Planteo Variacional del Problema

El objetivo del método de mínimos cuadrados es encontrar un vector estimado $\hat{x} \in \mathbb{R}^n$ tal que la distancia euclídea entre $Ax$ y $b$ sea mínima:
$$\min_{x \in \mathbb{R}^n} \|Ax - b\|^2 = \min_{x \in \mathbb{R}^n} (Ax - b)^T(Ax - b)$$

Geométricamente, el punto en $C(A)$ más cercano a $b$ es la proyección ortogonal $p = A\hat{x}$.

### Condición de Ortogonalidad y Deducción

El vector de error residual se define como $e = b - p = b - A\hat{x}$. Para minimizar la norma del error, el vector $e$ debe ser perpendicular a cada una de las columnas de $A$, lo que equivale a exigir que $e$ pertenezca al espacio nulo izquierdo $N(A^T)$:
$$A^T e = 0 \implies A^T (b - A\hat{x}) = 0$$

Despejando y reordenando los términos se obtiene el **Sistema de Ecuaciones Normales**:
$$A^T A \hat{x} = A^T b$$

### Solución Explícita y Matriz de Proyección

Si la matriz $A$ posee rango columna completo ($r = n$), la matriz de Gram $A^T A \in \mathbb{R}^{n \times n}$ es simétrica y definida positiva, por lo que es strictly invertible. La solución de mínimos cuadrados está dada por:
$$\hat{x} = (A^T A)^{-1} A^T b$$

Sustituyendo $\hat{x}$ en la fórmula de la proyección $p = A\hat{x}$:
$$p = A (A^T A)^{-1} A^T b = P b$$

donde $P = A (A^T A)^{-1} A^T \in \mathbb{R}^{m \times m}$ es la **Matriz de Proyección** (conocida como matriz sombrero o *hat matrix* $H$).

#### Propiedades Fundamentales de la Matriz de Proyección $P$:
1. **Idempotencia**: $P^2 = P$ (proyectar por segunda vez no altera el vector ya proyectado).
2. **Simetría**: $P^T = P$.
3. **Complemento ortogonal**: La matriz $I - P$ proyecta sobre el espacio nulo izquierdo $N(A^T)$, verificándose que $e = (I - P)b$.

---

## 3. Estabilidad Numérica y Factorización QR

El cálculo explícito de la matriz $A^T A$ y su posterior inversión presentan graves inconvenientes de estabilidad numérica en presencia de datos mal condicionados. Como señala Strang, el número de condición de $A^T A$ es el cuadrado del número de condición de $A$, $\kappa(A^T A) = \kappa(A)^2$, lo que amplifica drásticamente los errores de redondeo. La ortogonalización mediante la **Factorización QR** resuelve este problema operando directamente sobre $A$.

### Formulación de la Factorización QR

Toda matriz $A \in \mathbb{R}^{m \times n}$ con columnas linealmente independientes ($r = n$) se puede factorizar en la forma:
$$A = Q R$$

donde:
- $Q \in \mathbb{R}^{m \times n}$ es una matriz con columnas ortonormales que satisfacen $Q^T Q = I_n$.
- $R \in \mathbb{R}^{n \times n}$ es una matriz triangular superior con elementos diagonales estrictamente positivos ($r_{ii} > 0$).

### Resolución de Mínimos Cuadrados mediante QR

Sustituyendo $A = QR$ en el sistema de ecuaciones normales:
$$(QR)^T (QR) \hat{x} = (QR)^T b$$
$$R^T Q^T Q R \hat{x} = R^T Q^T b$$

Dado que $Q^T Q = I$:
$$R^T R \hat{x} = R^T Q^T b$$

Como $R^T$ es una matriz triangular inferior invertible, se pueden simplificar ambos miembros multiplicando por $(R^T)^{-1}$:
$$R \hat{x} = Q^T b$$

### Algoritmo de Cálculo Paso a Paso

1. **Ortogonalización de Gram-Schmidt (o Transformaciones de Householder)**:
   A partir de las columnas $a_1, a_2, \dots, a_n$ de $A$, se generan los vectores ortonormales $q_1, q_2, \dots, q_n$:
   $$v_j = a_j - \sum_{i=1}^{j-1} (q_i^T a_j) q_i, \quad q_j = \frac{v_j}{\|v_j\|}$$
2. **Construcción de $R$**:
   Los elementos de la matriz triangular $R$ se calculan como $r_{ij} = q_i^T a_j$ para $i \le j$.
3. **Sustitución Hacia Atrás**:
   Se computa el vector del lado derecho $d = Q^T b$ y se resuelve el sistema triangular superior $R \hat{x} = d$ por sustitución hacia atrás, evitando por completo la inversión de matrices.

---

## 4. Descomposición en Valores Singulares (SVD) y Seudoinversa

Cuando la matriz $A$ no posee rango columna completo ($r < n$), la matriz $A^T A$ es singular y el sistema de ecuaciones normales admite infinitas soluciones. En esta situación, Gilbert Strang presenta la Descomposición en Valores Singulares (SVD) como la herramienta más potente y completa del álgebra lineal aplicada.

### Teorema de la SVD

Cualquier matriz real $A \in \mathbb{R}^{m \times n}$ de rango $r$ se factoriza en la forma:
$$A = U \Sigma V^T$$

donde:
- $U \in \mathbb{R}^{m \times m}$ es una matriz ortogonal ($U^T U = I_m$) cuyas columnas son los **vectores singulares a izquierda** (autovectores de $A A^T$).
- $V \in \mathbb{R}^{n \times n}$ es una matriz ortogonal ($V^T V = I_n$) cuyas columnas son los **vectores singulares a derecha** (autovectores de $A^T A$).
- $\Sigma \in \mathbb{R}^{m \times n}$ es una matriz diagonal por bloques que contiene los **valores singulares** $\sigma_1 \ge \sigma_2 \ge \dots \ge \sigma_r > 0$, donde $\sigma_i = \sqrt{\lambda_i(A^T A)}$.

### Construcción de la Seudoinversa de Moore-Penrose ($A^+$)

La seudoinversa $A^+ \in \mathbb{R}^{n \times m}$ extiende el concepto de matriz inversa a cualquier matriz rectangular o de rango deficiente:
$$A^+ = V \Sigma^+ U^T$$

donde $\Sigma^+ \in \mathbb{R}^{n \times m}$ se obtiene transponiendo $\Sigma$ y reemplazando cada valor singular no nulo $\sigma_i$ por su recíproco $1/\sigma_i$:

$$\Sigma^+ = \begin{bmatrix} \frac{1}{\sigma_1} & & & 0 \\ & \ddots & & \vdots \\ & & \frac{1}{\sigma_r} & 0 \\ 0 & \dots & 0 & 0 \end{bmatrix}$$

### Solución de Norma Mínima en Mínimos Cuadrados

Para un sistema general $Ax = b$ con rango deficiente, el conjunto de soluciones que minimizan $\|Ax - b\|^2$ forma un subespacio afín. Entre todas esas soluciones, existe una única solución de **norma euclídea mínima** $\|x\|$, dada exactamente por:
$$x^+ = A^+ b = V \Sigma^+ U^T b$$

En términos de la expansión por columnas de la SVD reducida:
$$x^+ = \sum_{i=1}^r \frac{u_i^T b}{\sigma_i} v_i$$

Esta formulación garantiza que $x^+$ pertenezca estrictamente al espacio fila $C(A^T)$, siendo perpendicular al espacio nulo $N(A)$.

---

## 5. Formulación Matricial para el Ajuste de Modelos de Datos

Para aplicar el método de mínimos cuadrados a problemas de regresión con datos experimentales $\{(t_i, y_i); i = 1, 2, \dots, m\}$, es necesario expresar el modelo de ajuste en la forma matricial $Ax \approx b$.

### Modelos Polinómicos y Matriz de Vandermonde

Para ajustar un polinomio de grado $k$ ($m > k + 1$):
$$y(t) = C_0 + C_1 t + C_2 t^2 + \dots + C_k t^k$$

Se definen los vectores y la matriz de diseño de Vandermonde $A$:
$$b = \begin{bmatrix} y_1 \\ y_2 \\ \vdots \\ y_m \end{bmatrix}, \quad A = \begin{bmatrix} 1 & t_1 & t_1^2 & \dots & t_1^k \\ 1 & t_2 & t_2^2 & \dots & t_2^k \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ 1 & t_m & t_m^2 & \dots & t_m^k \end{bmatrix}, \quad x = \begin{bmatrix} C_0 \\ C_1 \\ \vdots \\ C_k \end{bmatrix}$$

El sistema normal $A^T A x = A^T b$ adopta la estructura explícita:
$$\begin{bmatrix} m & \sum t_i & \dots & \sum t_i^k \\ \sum t_i & \sum t_i^2 & \dots & \sum t_i^{k+1} \\ \vdots & \vdots & \ddots & \vdots \\ \sum t_i^k & \sum t_i^{k+1} & \dots & \sum t_i^{2k} \end{bmatrix} \begin{bmatrix} C_0 \\ C_1 \\ \vdots \\ C_k \end{bmatrix} = \begin{bmatrix} \sum y_i \\ \sum t_i y_i \\ \vdots \\ \sum t_i^k y_i \end{bmatrix}$$

### Linealización de Modelos No Lineales

Cuando la relación funcional entre la variable independiente $x$ y la respuesta $y$ no es lineal en los parámetros, se aplican transformaciones algebraicas para convertir el modelo en una forma lineal equivalente:

| Modelo Original | Transformación de Variables | Modelo Linealizado | Ecuaciones de los Parámetros |
| :--- | :--- | :--- | :--- |
| **Exponencial**: $y = C e^{D t}$ | $Y = \ln(y)$ | $Y = \ln(C) + D t$ | $C_0 = \ln(C) \implies C = e^{C_0}$, $C_1 = D$ |
| **Potencial**: $y = C t^D$ | $Y = \ln(y)$, $T = \ln(t)$ | $Y = \ln(C) + D T$ | $C_0 = \ln(C) \implies C = e^{C_0}$, $C_1 = D$ |
| **Tasa Racional / Cociente**: $y = \frac{t}{C + D t}$ | $Y = \frac{1}{y}$, $T = \frac{1}{t}$ | $Y = C T + D$ | $C_1 = C$, $C_0 = D$ |
| **Crecimiento Saturado**: $y = \frac{C t}{D + t}$ | $Y = \frac{1}{y}$, $T = \frac{1}{t}$ | $Y = \frac{1}{C} + \frac{D}{C} T$ | $C_0 = \frac{1}{C}$, $C_1 = \frac{D}{C}$ |

### Mínimos Cuadrados Ponderados (WLS)

Si las observaciones $y_i$ poseen varianzas desiguales $\sigma_i^2$ o covarianzas entre sí, la matriz de covarianza de los errores es $V$. La minimización del error pesado conduce a la norma ponderada:
$$\min_x (b - Ax)^T V^{-1} (b - Ax)$$

Lo que genera las **Ecuaciones Normales Ponderadas**:
$$(A^T V^{-1} A) \hat{x} = A^T V^{-1} b$$

---

## 6. Regularización y Métodos Avanzados para Grandes Volúmenes de Datos

En ciencia de datos moderna, la presencia de multicolinealidad o problemas mal condicionados exige incorporar términos de regularización para evitar el sobreajuste y estabilizar la solución.

### Regresión Ridge (Penalización $L_2$)

La regresión Ridge agrega una restricción a la magnitud del vector de parámetros $x$, modificando la función de costo:
$$\min_x \|Ax - b\|^2 + \delta^2 \|x\|^2$$

Al derivar e igualar a cero, se obtiene el sistema regularizado:
$$(A^T A + \delta^2 I) \hat{x}_\delta = A^T b$$

Dado que $\delta^2 > 0$, la matriz $(A^T A + \delta^2 I)$ es siempre estrictamente definida positiva e invertible, incluso si $A$ no posee rango completo.

#### Propiedad Límite de la Seudoinversa:
Strang demuestra que la seudoinversa de Moore-Penrose $A^+$ es el límite continuo de la solución Ridge cuando el factor de regularización tiende a cero:
$$A^+ = \lim_{\delta \to 0} (A^T A + \delta^2 I)^{-1} A^T$$

### SVD y Análisis de Componentes Principales (PCA)

Para una matriz de datos centrada $A_0 \in \mathbb{R}^{m \times n}$ (donde se ha restado la media a cada columna), la matriz de covarianza muestral es:
$$S = \frac{A_0^T A_0}{m - 1}$$

Aplicando la SVD a $A_0 = U \Sigma V^T$:
- Las columnas de $V$ (vectores singulares a derecha) son los **componentes principales** y representan las direcciones de máxima varianza.
- Los autovalores de la matriz de covarianza se conectan con los valores singulares mediante $\lambda_i = \frac{\sigma_i^2}{m - 1}$.
- El teorema de Eckart-Young establece que la mejor aproximación de rango $k < r$ para la matriz de datos en la norma de Frobenius y norma espectral es la truncada $A_k = \sum_{i=1}^k \sigma_i u_i v_i^T$.
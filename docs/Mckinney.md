# Resumen Completo y Técnico: Python for Data Analysis (2da Edición) — Wes McKinney

Este documento ofrece un resumen exhaustivo y detallado de la segunda edición del libro de referencia *Python for Data Analysis: Data Wrangling with Pandas, NumPy, and IPython* escrito por Wes McKinney, creador de la biblioteca pandas. El contenido abarca la totalidad de los 14 capítulos y los 2 apéndices del texto, ofreciendo un desarrollo de máxima profundidad analítica y técnica para los Capítulos 6, 7, 8 y 9.

---

## 1. Visión General del Libro y Ecosistema de Datos (Capítulos 1 a 3)

### Capítulo 1: Preliminares y Bibliotecas Fundamentales
El texto establece las bases conceptuales y herramientas para manipular, procesar, limpiar y analizar datos estructurados en Python. Se define el concepto de datos estructurados como aquellos conjuntos tabulares (similares a tablas de bases de datos relacionales u hojas de cálculo), matrices multidimensionales, tablas interrelacionadas mediante claves y series temporales uniformes o irregulares.

Las bibliotecas esenciales descritas constituyen los pilares del análisis de datos en Python:
* **NumPy (Numerical Python)**: Es la base del cálculo numérico en Python. Proporciona el objeto `ndarray`, un arreglo multidimensional de alto rendimiento que permite operaciones vectorizadas y almacenamiento contiguo en memoria, además de funciones para álgebra lineal, generación de números aleatorios y transformadas de Fourier.
* **pandas**: Proporciona estructuras de datos de alto nivel con índices etiquetados (`Series` y `DataFrame`) diseñadas para manipular datos tabulares y heterogéneos de forma rápida y expresiva. Integra capacidades relacionales (similares a SQL) y soporte avanzado para series temporales.
* **matplotlib**: Es la librería fundamental para la generación de gráficos y visualizaciones bidimensionales estáticas con calidad de publicación.
* **IPython y Jupyter**: Entornos de ejecución interactivos. IPython ofrece una consola mejorada para exploración de datos y depuración, mientras que Jupyter Notebook proporciona una interfaz web interactiva basada en celdas de código y texto narrativo.
* **SciPy**: Colección de módulos científicos construida sobre NumPy que incluye algoritmos para integración numérica, optimización, procesamiento de señales y distribuciones estadísticas.
* **scikit-learn**: Kit de herramientas para aprendizaje automático en Python que cubre algoritmos de clasificación, regresión, clustering, reducción de dimensionalidad y selección de modelos.
* **statsmodels**: Paquete orientado a la estimación de modelos estadísticos, econometría y realización de pruebas de hipótesis.

### Capítulo 2: Bases del Lenguaje Python, IPython y Jupyter
Aborda los aspectos mecánicos del lenguaje Python y los patrones de desarrollo interactivo:
* **Herramientas de Desarrollo**: Uso de la consola IPython y los cuadernos Jupyter. Destaca la función del autocompletado con la tecla `Tab`, la introspección de objetos mediante `?` y `??`, la ejecución de scripts externos mediante `%run`, la captura de tiempos de ejecución con `%time` y `%timeit`, y la configuración de gráficos interactivos con `%matplotlib inline` o `%matplotlib notebook`.
* **Semántica del Lenguaje**: Destaca el tipado dinámico pero fuerte, las referencias mutables e inmutables, la indentación estricta en lugar de llaves delimitadoras y el uso de tipos escalares fundamentales (`int`, `float`, `str`, `bool`, `bytes`).
* **Control de Flujo**: Estructuras de decisión condicional (`if-elif-else`), iteraciones mediante bucles `for` y `while`, y la sintaxis abreviada de expresiones ternarias (`valor = expr_true if condicion else expr_false`).

### Capítulo 3: Estructuras de Datos Integradas, Funciones y Archivos
Examina las estructuras de datos nativas de Python indispensables para la preparación previa de los datos:
* **Secuencias Nativas**:
  * `tuple`: Secuencia inmutable de longitud fija. Admite desempaquetado de elementos (`a, b = (1, 2)`).
  * `list`: Secuencia mutable de longitud variable (`append`, `insert`, `pop`, `sort`, rebanado o *slicing*).
  * `dict`: Estructura de par clave-valor basada en tablas hash (`keys()`, `values()`, `get()`, `setdefault()`).
  * `set`: Colección no ordenada de elementos únicos orientada a operaciones de teoría de conjuntos (unión `|`, intersección `&`, diferencia `-`).
* **Comprensiones**: Sintaxis concisa para generar listas, diccionarios y conjuntos (`[expr for val in collection if condition]`).
* **Funciones**: Creación de funciones reutilizables con argumentos posicionales y de palabra clave, espacios de nombres locales y globales, funciones anónimas `lambda`, generadores basados en `yield` y manejo de excepciones mediante estructuras `try-except-finally`.
* **Archivos y Sistema Operativo**: Manejo de objetos de archivo con `open()`, lectura de líneas (`readline`, `readlines`), escritura de texto y codificaciones Unicode (UTF-8).

---

## 2. Fundamentos de Cálculo Numérico con NumPy (Capítulo 4)

El arreglo multidimensional `ndarray` constituye el contenedor fundamental de datos numéricos en Python. Permite aplicar operaciones matemáticas elementales sobre bloques completos de memoria sin recurrir a bucles `for` del intérprete.

### Estructura y Operaciones sobre `ndarray`
* **Vectorización y Tipos de Datos (`dtype`)**: El arreglo `ndarray` almacena datos homogéneos en bloques de memoria contiguos. Los tipos de datos se especifican explícitamente (`float64`, `int32`, `bool`, `complex128`).
* **Indexación y Rebanado (*Slicing*)**:
  * Indexación básica: Genera vistas (*views*) del arreglo original sin duplicar datos en memoria.
  * Indexación booleana: Filtra elementos mediante evaluaciones lógicas (`arr[arr > 0]`).
  * Indexación elegante (*Fancy Indexing*): Transmite arreglos de números enteros para extraer subconjuntos en órdenes específicos.
* **Funciones Universales (`ufunc`)**: Operaciones elementales rápidas aplicadas punto a punto sobre el arreglo (`np.sqrt`, `np.exp`, `np.maximum`, `np.add`).
* **Programación Orientada a Arreglos**: Expresión de lógicas condicionales vectorizadas con `np.where`, operaciones lógicas sobre arreglos booleanos (`any()`, `all()`), ordenamiento (`sort`), métodos estadísticos (`mean()`, `sum()`, `std()`, `cumsum()`) y operaciones de teoría de conjuntos (`np.unique`, `np.in1d`).
* **Álgebra Lineal y Números Aleatorios**: Multiplicación de matrices mediante `np.dot` o el operador `@`, descomposiciones matriciales (`np.linalg.svd`, `np.linalg.inv`) y generación de arreglos aleatorios bajo distribuciones estadísticas (`numpy.random`).

---

## 3. Estructuras de Datos Primarias de pandas (Capítulo 5)

pandas extiende las capacidades numéricas de NumPy ofreciendo estructuras con índices etiquetados preparadas para manejar datos tabulares y heterogéneos.

### Estructuras Core
1. **Series**: Arreglo unidimensional etiquetado capaz de albergar cualquier tipo de dato. Se compone de un arreglo de datos y un arreglo de etiquetas asociado denominado `index`.
2. **DataFrame**: Estructura tabular bidimensional con filas y columnas etiquetadas. Conceptualmente equivale a un diccionario de objetos `Series` que comparten un mismo índice de filas.

### Funcionalidades Esenciales
* **Reindexación (`reindex`)**: Adapta un objeto a un nuevo índice, insertando valores faltantes `NaN` donde las etiquetas no existan previamente o aplicando métodos de interpolación (`ffill`, `bfill`).
* **Selección Explícita con `loc` e `iloc`**:
  * `loc`: Selección basada estrictamente en etiquetas de filas y nombres de columnas.
  * `iloc`: Selección basada en posiciones enteras de índice (de $0$ a $N-1$).
* **Alineación de Datos y Aritmética**: Operaciones entre objetos con índices dispares realizan automáticamente la unión de etiquetas (similar a un *outer join*), asignando `NaN` a las discrepancias. Métodos con valores de relleno: `df.add(df2, fill_value=0)`.
* **Estadísticas Descriptivas**: Agregaciones que excluyen valores faltantes por defecto (`mean`, `sum`, `std`, `var`, `cumsum`, `describe()`). Funciones para cálculo de correlación y covarianza (`corr()`, `cov()`) y cuantificación de frecuencias (`value_counts()`).

---

## 4. Capítulo 6 (Detallado): Carga de Datos, Almacenamiento y Formatos de Archivo

El acceso a datos externos es el primer paso en la mayoría de las aplicaciones de análisis. pandas dispone de un conjunto amplio de funciones de lectura y escritura diseñadas para transformar datos desde archivos de texto, formatos binarios, APIs web y bases de datos relacionales hacia objetos `DataFrame`.

### 4.1 Lectura y Escritura de Datos en Formato de Texto

Las funciones `read_csv` y `read_table` representan las herramientas principales para importar archivos de texto delimitados.

La siguiente tabla resume las funciones de lectura de texto plano incorporadas en pandas:

| Función | Descripción de la Operación | Separador Predeterminado |
| :--- | :--- | :--- |
| `read_csv` | Carga datos delimitados desde un archivo, URL u objeto ejecutable | Coma (`,`) |
| `read_table` | Carga datos delimitados especificando el separador mediante parámetros | Tabulador (`\t`) |
| `read_fwf` | Lee datos en formato de columnas de ancho fijo (sin delimitadores) | N/A |
| `read_clipboard` | Variante de `read_table` que lee datos directamente desde el portapapeles | Tabulador (`\t`) |

#### Argumentos Clave de Configuración en el Parsing
* **Indexación (`index_col`)**: Define la columna o lista de columnas que constituirán el índice del `DataFrame` (permitiendo construir índices jerárquicos).
* **Nombres de Columnas (`header`, `names`)**: `header=None` indica que el archivo no posee fila de encabezado; `names` suministra una lista explícita de nombres para las columnas.
* **Manejo de Valores Faltantes (`na_values`, `keep_default_na`)**: Recibe una cadena, lista o diccionario con los valores centinela por columna que deben interpretarse como `NaN`.
* **Ignorar Filas (`skiprows`, `skipfooter`)**: Omite un número determinado de filas al inicio o final del archivo, o una lista con los números de línea exactos a descartar.
* **Parsing de Fechas (`parse_dates`, `date_parser`)**: Convierte automáticamente columnas a objetos `datetime64`. Si se pasa una lista de tuplas, combina múltiples columnas separadas (como fecha y hora) en una única columna procesada.

#### Lectura de Archivos Masivos por Bloques (*Chunking*)
Al procesar archivos de gran tamaño que superan la memoria disponible, el parámetro `chunksize` retorna un objeto iterador de tipo `TextFileReader`:

```python
chunker = pd.read_csv('archivo_gigante.csv', chunksize=10000)
tot = pd.Series([], dtype='float64')
for piece in chunker:
    tot = tot.add(piece['categoria'].value_counts(), fill_value=0)
```

Por su parte, el parámetro `nrows` permite leer únicamente un número reducido de filas iniciales para inspeccionar la estructura del archivo antes de ejecutar el parsing completo.

#### Escritura a Formatos Delimitados
El método de instancia `to_csv` exporta los datos a un archivo de texto. Permite configurar el delimitador (`sep`), la representación sintáctica de los valores nulos (`na_rep='NULL'`), y omitir la exportación de los índices de fila o columna (`index=False`, `header=False`).

### 4.2 Formatos Delimitados, JSON, XML y HTML

#### El Módulo Nativo `csv`
Para procesar archivos malformados con registros irregulares que interrumpen a `read_csv`, el módulo estándar de Python `csv.reader` e `csv.writer` permite iterar línea por línea procesando tuplas de cadenas con dialectos personalizados (`Dialect`).

#### JSON (JavaScript Object Notation)
JSON almacena datos no relacionales en cadenas de texto plano.
* **Importación**: `pd.read_json` convierte arreglos u objetos JSON directamente a marcos `DataFrame`.
* **Exportación**: `df.to_json(orient='records')` serializa la estructura tabular en una secuencia de registros JSON.

#### Web Scraping y Parsing de XML/HTML
* `pd.read_html`: Extrae automáticamente todas las tablas contenidas dentro de etiquetas `<table>` en documentos HTML utilizando internamente `lxml` o `BeautifulSoup`, retornando una lista de objetos `DataFrame`.
* `lxml.objectify`: Permite estructurar y navegar documentos XML jerárquicos procesando los nodos mediante métodos como `getroot()`, `get()` y `find()`.

### 4.3 Formatos Binarios de Almacenamiento

Los formatos binarios son significativamente más rápidos en operaciones de entrada/salida y preservan con exactitud los tipos de datos nativos sin sufrir pérdidas por conversión a texto.

#### Serialización con `pickle`
El método `to_pickle` y la función `read_pickle` permiten almacenar objetos de pandas en disco mediante el módulo de serialización nativo de Python. Es importante considerar que el formato `pickle` no garantiza estabilidad entre versiones distantes de bibliotecas, por lo que se recomienda para almacenamiento temporal a corto plazo.

#### Formato HDF5 (`HDFStore`)
HDF5 (Hierarchical Data Format) es una tecnología orientada al almacenamiento de grandes volúmenes de datos numéricos científicos con compresión integrada.
* `HDFStore`: Funciona como un diccionario en disco donde cada clave contiene un objeto de pandas.
* **Formatos de Almacenamiento**:
  * `fixed`: Formato rígido de rápida lectura pero no modificable posteriormente.
  * `table`: Formato estructurado appendable que permite realizar consultas e indexación directa en disco mediante expresiones lógicas (`where=['index < 5000']`).

```python
store = pd.HDFStore('mis_datos.h5')
store.put('tabla_1', df, format='table', data_columns=True)
resultado = store.select('tabla_1', where=['index < 100'])
store.close()
```

#### Apache Feather y Microsoft Excel
* **Feather**: Formato de almacenamiento en columnas diseñado en conjunto con la comunidad de R que utiliza la memoria orientada a columnas de Apache Arrow.
* **Archivos Excel**: `pd.ExcelFile` y `pd.read_excel` importan hojas de cálculo `.xls` y `.xlsx` apoyándose en las librerías `xlrd` u `openpyxl`. La exportación se realiza mediante `pd.ExcelWriter` y el método `to_excel()`.

### 4.4 Interacción con APIs Web y Bases de Datos SQL

#### Peticiones a APIs Web
Haciendo uso de la biblioteca `requests`, se realizan peticiones HTTP (`GET`) a servicios web que retornan respuestas en formato JSON para transformarlas directamente en marcos de datos:

```python
import requests
response = requests.get('https://api.github.com/repos/pandas-dev/pandas/issues')
data = response.json()
issues = pd.DataFrame(data, columns=['number', 'title', 'state'])
```

#### Integración con SQL mediante SQLAlchemy
pandas simplifica la carga de datos relacionales mediante la función `read_sql`, la cual se conecta a bases de datos relacionales utilizando la abstracción de `SQLAlchemy` o controladores nativos como `sqlite3`:

```python
import sqlalchemy as sqla
db = sqla.create_engine('sqlite:///mi_base.sqlite')
df = pd.read_sql('SELECT * FROM ventas WHERE monto > 1000', db)
df.to_sql('ventas_filtradas', db, if_exists='replace', index=False)
```

---

## 5. Capítulo 7 (Detallado): Limpieza y Preparación de Datos

El proceso de preparación de datos (que incluye la limpieza, transformación, filtrado y reorganización) suele requerir la mayor parte del tiempo en un proyecto analítico. pandas ofrece herramientas de alto nivel para abordar las imperfecciones en los datos.

### 5.1 Manejo de Valores Faltantes

En arreglos numéricos, pandas representa los datos faltantes mediante el centinela flotante `NaN` (Not a Number) o el objeto `None` en columnas de tipo objeto.

#### Detección y Filtrado de Faltantes
* **Detección**: `isnull()` y `notnull()` devuelven estructuras booleanas con el estado de cada celda.
* **Filtrado (`dropna`)**:
  * En objetos `Series`: Devuelve únicamente las observaciones no nulas.
  * En objetos `DataFrame`: Elimina por defecto cualquier fila que contenga al menos un valor faltante (`axis=0`).
  * `how='all'`: Elimina la fila o columna solo si todos sus registros son `NaN`.
  * `thresh=n`: Mantiene las filas o columnas que posean al menos $n$ valores válidos.

#### Imputación o Relleno (`fillna`)
Permite sustituir los valores nulos por estimaciones o valores constantes:
* **Valores Constantes**: `df.fillna(0)` o diccionarios con valores específicos por columna `df.fillna({'A': 0, 'B': 0.5})`.
* **Interpolación Secuencial**: `method='ffill'` (propaga el último valor válido hacia adelante) o `method='bfill'` (hacia atrás), con un parámetro `limit` para restringir sustituciones consecutivas.
* **Estadísticos Calculados**: `df.fillna(df.mean())`.

### 5.2 Transformación de Datos

#### Eliminación de Duplicados
* `duplicated()`: Retorna una serie booleana indicando si una fila es idéntica a una presentada anteriormente.
* `drop_duplicates()`: Elimina las filas duplicadas. Acepta una lista de columnas específicas y el parámetro `keep='last'` para conservar la última ocurrencia en lugar de la primera.

#### Transformación Mediante Funciones o Mapeos (`map`)
El método `map` aplica un diccionario de correspondencias o una función sobre una objeto `Series` para construir columnas derivadas (por ejemplo, mapear tipos de alimentos con sus respectivos animales de origen).

#### Reemplazo de Valores (`replace`)
Sustituye valores atípicos o centinelas específicos por valores corregidos o por `NaN`:

```python
s.replace([-999, -1000], [np.nan, 0])
s.replace({-999: np.nan, -1000: 0})
```

#### Renombrado de Etiquetas de Ejes (`rename`)
Permite alterar las etiquetas de los índices de fila o columna sin modificar los datos internos:

```python
df.rename(index=str.upper, columns={'col_antigua': 'col_nueva'}, inplace=True)
```

#### Discretización y Binning (`cut`, `qcut`)
Los datos continuos suelen segmentarse en intervalos discretos o contenedores (*bins*):
* `cut`: Segmenta los datos utilizando límites numéricos explícitos o dividiendo el rango en $n$ intervalos de igual ancho.

```python
edades = [20, 22, 25, 27, 21, 23, 37, 31, 61, 45]
bins = [18, 25, 35, 60, 100]
categorias = pd.cut(edades, bins, labels=['Joven', 'AdultoJoven', 'Adulto', 'Senior'])
```

* `qcut`: Discretiza la variable basándose en los cuantiles muestrales (percentiles), garantizando que cada contenedor albergue una cantidad equivalente de observaciones.

#### Detección de Outliers (Valores Atípicos)
Se realiza mediante comparaciones vectoriales sobre arreglos numéricos. Por ejemplo, identificar filas donde el valor absoluto supere 3 desviaciones estándar:

```python
outliers = df[(np.abs(df) > 3).any(axis=1)]
df[np.abs(df) > 3] = np.sign(df) * 3  # Truncamiento o capping
```

#### Muestreo Aleatorio y Permutaciones
`numpy.random.permutation` reordena aleatoriamente los índices de un eje. El método `sample` sobre `DataFrame` permite extraer muestras aleatorias directas con o sin reemplazo (`replace=True`).

#### Variables Dummy / One-Hot Encoding (`get_dummies`)
Convierte variables categóricas en matrices binarias ($0$ o $1$) donde cada columna representa una categoría única. Es una etapa esencial en la preparación de datos para modelos de aprendizaje automático.

### 5.3 Manipulación de Cadenas y Expresiones Regulares

pandas extiende las capacidades nativas de procesamiento de texto de Python para aplicarlas vectorialmente sobre arreglos ignorando valores faltantes.

#### Métodos Nativos de Cadenas
Métodos de objeto como `strip()`, `split()`, `join()`, `find()`, `replace()`, `lower()` y `upper()` operan elemento por elemento sobre secuencias de texto.

#### Expresiones Regulares (Módulo `re`)
Ofrece herramientas para búsqueda y manipulación de patrones de texto:
* `re.compile`: Compila la regla regex para optimizar búsquedas repetitivas.
* `findall()`: Retorna todas las coincidencias del patrón encontradas en el texto.
* `search()`: Retorna un objeto de coincidencia (`Match`) con la primera ocurrencia.
* `match()`: Evalúa si la cadena inicia cumpliendo la regla definida.
* `sub()`: Reemplaza las coincidencias encontradas por una nueva cadena.

#### Funciones Vectorizadas de Cadenas en pandas (`.str`)
El acceso a la propiedad `.str` en un objeto `Series` permite invocar métodos de cadenas y expresiones regulares de manera segura sobre toda la columna:

```python
pattern = r'([A-Z0-9._%+-]+)@([A-Z0-9.-]+)\.([A-Z]{2,4})'
s.str.extract(pattern, flags=re.IGNORECASE)  # Extrae grupos regex a columnas
s.str.contains('gmail')  # Retorna máscara booleana de coincidencia
s.str[:5]  # Rebanado sintáctico de texto
```

---

## 6. Capítulo 8 (Detallado): Manipulación de Datos: Join, Combine y Reshape

El análisis aplicado requiere frecuentemente fusionar múltiples tablas, realizar cruces basados en llaves relacionales o reestructurar la disposición de filas y columnas.

### 6.1 Indexación Jerárquica (MultiIndex)

La indexación jerárquica permite trabajar con múltiples niveles de índices sobre un mismo eje, proporcionando una vía para representar datos de dimensiones superiores en estructuras bidimensionales.

```python
frame = pd.DataFrame(np.arange(12).reshape((4, 3)),
                     index=[['a', 'a', 'b', 'b'], [1, 2, 1, 2]],
                     columns=[['Ohio', 'Ohio', 'Colorado'], ['Green', 'Red', 'Green']])
frame.index.names = ['nivel_1', 'nivel_2']
frame.columns.names = ['estado', 'color']
```

#### Operaciones sobre Niveles Jerárquicos
* **Reordenamiento**: `swaplevel('nivel_1', 'nivel_2')` intercambia las posiciones de los niveles. `sort_index(level=1)` ordena las filas basándose en el nivel especificado.
* **Estadísticas por Nivel**: Operaciones como `frame.sum(level='nivel_2')` o `frame.sum(level='color', axis=1)` agregan la información colapsando dimensiones específicas.
* **Transferencia entre Ejes**: `set_index(['c1', 'c2'])` transforma columnas ordinarias en un índice jerárquico de filas. Su operación inversa, `reset_index()`, convierte los niveles del índice en columnas estándar.

### 6.2 Combinación y Fusión de Conjuntos de Datos

pandas proporciona distintos algoritmos para conectar objetos `Series` y `DataFrame`.

#### Combinaciones Relacionales (`pd.merge`)
Conecta filas en marcos de datos basándose en una o más llaves coincidentes (equivalente a las operaciones *Join* en SQL).

```python
pd.merge(df1, df2, on='clave', how='inner')
```

La siguiente tabla describe los comportamientos del parámetro `how`:

| Tipo de Join (`how`) | Comportamiento en la Combinación |
| :--- | :--- |
| `'inner'` | Conserva únicamente las combinaciones de llaves presentes en ambas tablas. |
| `'left'` | Mantiene todas las filas de la tabla izquierda y completa discrepancias con `NaN`. |
| `'right'` | Mantiene todas las filas de la tabla derecha y completa discrepancias con `NaN`. |
| `'outer'` | Realiza la unión completa de llaves, conservando todas las filas de ambos lados. |

* **Llaves con Nombres Distintos**: `left_on='key1'`, `right_on='key2'`.
* **Sufijos de Columna**: `suffixes=('_izq', '_der')` resuelve conflictos de nombres cuando existen columnas no clave idénticas en ambas tablas.

#### Fusión por Índice (`left_index`, `right_index`)
Si la llave de combinación se encuentra en el índice de fila de una o ambas tablas, se especifican las opciones `left_index=True` y/o `right_index=True`. El método de instancia `df1.join(df2)` ofrece un atajo simplificado para unir tablas por índice (ejecutando un *left join* por defecto).

#### Concatenación a lo Largo de un Eje (`pd.concat`)
Apila objetos a lo largo de un eje determinado (`axis=0` para apilar filas, `axis=1` para apilar columnas).
* **Intersección vs Unión**: `join='inner'` conserva únicamente los índices comunes del eje opuesto; `join='outer'` realiza la unión de todos ellos.
* **Identificación de Bloques**: El parámetro `keys` crea un índice jerárquico que permite identificar el origen de cada bloque concatenado.
* **Reinicio de Índices**: `ignore_index=True` descarta los índices originales y genera un `RangeIndex` continuo.

#### Combinación de Datos con Solapamiento (`combine_first`)
Permite reemplazar valores faltantes de un objeto utilizando observaciones válidas de otro objeto con índices coincidentes o parciales (equivalente a la lógica `np.where(pd.isnull(a), b, a)`).

### 6.3 Reorganización y Pivoteo (*Reshaping & Pivoting*)

#### Apilado y Desapilado (`stack`, `unstack`)
* `stack()`: Mueve las columnas del `DataFrame` a un nivel interno del índice de filas.
* `unstack()`: Operación inversa que desplaza niveles del índice de filas hacia la dimensión de las columnas.

#### Pivoteo de Formato "Largo" a "Ancho" (`pivot`)
Las series temporales y datos observacionales se almacenan habitualmente en formato "largo" (una fila por cada combinación de clave y variable). `pivot` reorganiza la tabla para presentar una columna por cada variable independiente:

```python
# Convierte una tabla en formato largo a formato ancho
ancho = largo.pivot(index='fecha', columns='variable', values='valor')
```

#### Pivoteo de Formato "Ancho" a "Largo" (`pd.melt`)
Es la operación inversa a `pivot`. Consolida múltiples columnas de datos en una sola columna con pares variable-valor, incrementando la longitud del marco de datos:

```python
melted = pd.melt(df, id_vars=['key'], value_vars=['A', 'B'], var_name='variable', value_name='valor')
```

---

## 7. Capítulo 9 (Detallado): Visualización y Gráficos

La generación de representaciones visuales informativas es una etapa clave en la exploración de datos y la comunicación de hallazgos.

### 7.1 Primer de la API de matplotlib

matplotlib es la librería primaria de trazado bidimensional en Python. Su convención de importación estándar es `import matplotlib.pyplot as plt`.

#### Estructura de una Figura
Los trazados en matplotlib residen dentro de un objeto `Figure`. Las gráficas individuales se construyen sobre objetos `Subplot` o `AxesSubplot`.

```python
fig = plt.figure(figsize=(10, 6))
ax1 = fig.add_subplot(2, 2, 1)  # Malla 2x2, primer cuadrante
ax2 = fig.add_subplot(2, 2, 2)
ax3 = fig.add_subplot(2, 2, 3)
```

Atajo simplificado para generar la malla de subplots: `fig, axes = plt.subplots(2, 2, sharex=True, sharey=True)`.

#### Ajuste de Espaciado
`plt.subplots_adjust(wspace=0.2, hspace=0.2)` modifica la distancia entre subgráficos.

#### Estilos de Línea, Marcadores y Colores
La función `plot` acepta cadenas abreviadas o parámetros explícitos de configuración:

```python
ax.plot(x, y, color='k', linestyle='dashed', marker='o', label='Serie A')
```

#### Títulos, Ejes y Leyendas
* **Configuración de Ejes**: `ax.set_xticks([0, 250, 500])`, `ax.set_xticklabels(['Inicio', 'Medio', 'Fin'], rotation=30)`, `ax.set_title()`, `ax.set_xlabel()`.
* **Configuración en Bloque**: `ax.set(title='Mi Gráfico', xlabel='Etapa')`.
* **Leyendas**: `ax.legend(loc='best')` genera automáticamente la leyenda interpretando el parámetro `label` asignado a cada serie.

#### Anotaciones y Elementos Geométricos
* **Anotaciones**: `ax.text(x, y, 'Texto')` y `ax.annotate('Evento', xy=(x_obs, y_obs), xytext=(x_txt, y_txt), arrowprops=dict(facecolor='black'))`.
* **Parches Geométricos (`matplotlib.patches`)**: Inserción de objetos `Rectangle`, `Circle` o `Polygon` mediante `ax.add_patch(shp)`.

#### Exportación de Gráficos
`plt.savefig('grafico.png', dpi=400, bbox_inches='tight')` guarda la figura activa en disco recortando bordes no deseados.

### 7.2 Gráficos con pandas y seaborn

`pandas` incluye métodos integrados que interpretan automáticamente índices y columnas de Series y DataFrames. `seaborn` es una biblioteca de nivel superior que ofrece estilos visuales mejorados y abstrae agregaciones estadísticas complejas.

#### Tipos de Gráficos Principales

##### 1. Gráficos de Líneas
`Series.plot()` y `DataFrame.plot()` grafican cada columna como una línea independiente con leyenda integrada.

##### 2. Gráficos de Barras
* Verticaces (`plot.bar()`) y Horizontales (`plot.barh()`).
* **Barras Apiladas**: `stacked=True`.
* **Seaborn**: `sns.barplot(x='col_x', y='col_y', hue='categoria', data=df)` calcula automáticamente promedios e intervalos de confianza del 95% representados mediante barras de error.

```python
sns.barplot(x='tip_pct', y='day', hue='time', data=tips, orient='h')
```

##### 3. Histogramas y Gráficos de Densidad (KDE)
* **Histogramas**: `Series.plot.hist(bins=50)`.
* **Densidad (KDE)**: `Series.plot.density()`.
* **Seaborn**: `sns.distplot(values, bins=100, color='k')` representa simultáneamente el histograma de frecuencias y la estimación de densidad continua.

##### 4. Gráficos de Dispersión (*Scatter Plots*) y Matrices de Pares
* `sns.regplot('x', 'y', data=df)`: Dibuja un gráfico de dispersión con una recta de regresión lineal ajustada y su banda de confianza.
* `sns.pairplot(df, diag_kind='kde')`: Genera una matriz de dispersión (*scatter plot matrix*) comparando todas las combinaciones numéricas del marco de datos e insertando histogramas/KDEs en la diagonal principal.

##### 5. Mallas de Facetas (*Facet Grids*)
Permiten visualizar relaciones condicionales entre múltiples variables categóricas. `sns.factorplot` (o `catplot`) segmenta los gráficos en cuadrículas de filas y columnas:

```python
sns.factorplot(x='day', y='tip_pct', hue='time', col='smoker', kind='bar', data=tips)
sns.factorplot(x='tip_pct', y='day', kind='box', data=tips)  # Diagramas de caja (Box plots)
```

---

## 8. Operaciones Avanzadas y Agregación de Datos (Capítulos 10 a 12)

### Capítulo 10: Agregación de Datos y Operaciones de Grupo

Implementa la metodología **Split-Apply-Combine** (Dividir-Aplicar-Combinar):
1. **Split**: Los datos de un `DataFrame` o `Series` se dividen en grupos basándose en una o más llaves.
2. **Apply**: Se aplica una función a cada grupo de forma independiente.
3. **Combine**: Los resultados de cada grupo se consolidan en un único objeto de salida.

#### Operaciones Clave en GroupBy
* **Agregaciones Estándar**: `mean()`, `sum()`, `count()`, `std()`, `min()`, `max()`. Agregaciones personalizadas mediante `agg(func)`.
* **Agregaciones Múltiples**: `df.groupby('key').agg(['mean', 'std', ('rango', lambda x: x.max() - x.min())])`.
* **Transformación (`transform`)**: Aplica cálculos a nivel de grupo devolviendo un objeto con la misma dimensión que el original (por ejemplo, estandarizar o sustraer la media del grupo a cada fila).
* **Tablas Dinámicas (`pivot_table`)**: Resumen de datos agrupados por filas y columnas con opción de subtotales parciales (`margins=True`).
* **Tablas Cruzadas (`crosstab`)**: Cálculo de frecuencias absolutas o relativas entre variables categóricas.

### Capítulo 11: Series Temporales

Herramientas para la manipulación de datos temporales indexados de frecuencia fija o irregular:
* **Tipos de Datos**: `Timestamp` (instantes de tiempo), `DatetimeIndex`, `Period` (intervalos de tiempo) y `Timedelta`.
* **Indexación y Selección**: Filtros basados en cadenas temporales (`df['2017-05']`, `df['2015-01-01':'2015-12-31']`).
* **Generación de Rangos y Frecuencias**: `pd.date_range(start, end, freq='BM')` (frecuencias diarias, mensuales `M`, días hábiles `B`, etc.).
* **Desplazamientos (*Shifting*)**: `shift(1)` desplaza observaciones en el tiempo conservando el índice (útil para calcular variaciones).
* **Zonas Horarias**: Conversión y localización mediante la librería `pytz` (`tz_localize('UTC')`, `tz_convert('America/New_York')`).
* **Muestreo (*Resampling*)**:
  * Downsampling: Reducción de frecuencia (de diario a mensual) aplicando agregaciones (`df.resample('M').mean()`, `OHLC`).
  * Upsampling: Incremento de frecuencia mediante interpolación.
* **Ventanas Móviles (*Rolling Windows*)**: `rolling(window=30).mean()` para promedios móviles, promedios ponderados exponenciales (`ewm`).

### Capítulo 12: pandas Avanzado

Profundiza en la optimización del rendimiento y patrones de código encadenado:
* **Tipo Categórico (`Categorical`)**: Transforma columnas de texto repetitivo en códigos enteros asociados a una tabla de dimensión de categorías. Reduce drásticamente el uso de memoria RAM y acelera el rendimiento de las agrupaciones.
* **Encadenamiento de Métodos**:
  * `DataFrame.assign`: Asignación funcional de columnas que retorna un nuevo marco de datos para continuar el encadenamiento de métodos.
  * `pipe(func, *args)`: Permite integrar funciones personalizadas dentro de secuencias encadenadas de métodos.

---

## 9. Modelado y Casos de Estudio Prácticos (Capítulos 13 y 14)

### Capítulo 13: Introducción a Librerías de Modelado en Python

Construye el puente entre la preparación de datos realizada en pandas y la estimación de modelos estadísticos o algoritmos de Machine Learning.

#### Interfaz con Arreglos
La propiedad `.values` o el método `.to_numpy()` extraen arreglos `ndarray` homogéneos desde un `DataFrame` para alimentar modelos externos.

#### Construcción de Matrices con Patsy
La librería `patsy` permite describir modelos estadísticos utilizando expresiones de fórmulas tipo R (`y ~ x0 + x1 + categoria`). Transforma automáticamente variables categóricas en columnas binarias e inserta términos de intercepto.

#### Integración con `statsmodels` y `scikit-learn`
* **statsmodels**: Estimación de modelos lineales mediante Mínimos Cuadrados Ordinarios (`smf.ols`) y ajuste de modelos de series temporales (ARIMA).
* **scikit-learn**: Flujo estandarizado mediante estimadores (`fit()`, `predict()`, `transform()`) e integración con validación cruzada.

### Capítulo 14: Ejemplos de Análisis de Datos en el Mundo Real

Aplica de forma integrada las herramientas del libro mediante 5 casos de estudio basados en datos reales:

1. **Datos de 1.USA.gov desde Bitly**: Parsing de registros JSON, conteo de zonas horarias principales e identificación de sistemas operativos (Windows vs. no Windows) mediante transformaciones de cadenas e histogramas de barras.
2. **Dataset MovieLens 1M**: Carga y fusión (*merge*) de 1 millón de calificaciones, datos de usuarios y metadatos de películas distribuidos en 3 tablas. Uso de tablas dinámicas para evaluar diferencias en puntuaciones entre géneros.
3. **Nombres de Bebés en EE.UU. (1880–2010)**: Consolidación de 131 archivos CSV anuales. Análisis de tendencias de popularidad, cálculo de diversidad de nombres, evolución de letras finales y análisis de nombres ambivalentes por género.
4. **Base de Datos de Nutrientes de la USDA**: Limpieza y unificación de estructuras JSON compuestas con listas anidadas de nutrientes. Comparación de aportes nutricionales promedio por grupo alimenticio.
5. **Base de Datos de la Elección Federal de EE.UU. 2012**: Análisis de contribuciones de campaña electoral. Limpieza de descripciones de empleo/ocupación, filtrado de donaciones positivas y análisis de montos por candidato, ocupación y estado.

---

## 10. Apéndices Técnicos: NumPy Avanzado e IPython

### Apéndice A: NumPy Avanzado
* **Estructura del `ndarray`**: Estrides (*strides*), diseño de memoria en orden C (por filas) vs. Fortran (por columnas), y vistas sin duplicación de memoria (*zero-copy views*).
* **Broadcasting**: Reglas para ejecutar operaciones aritméticas entre arreglos de dimensiones diferentes pero compatibles.
* **Arreglos Estructurados**: Definición de registros heterogéneos similares a estructuras de C o filas de SQL.
* **Compilación con Numba**: Uso del decorador `@jit` para compilar funciones de Python a código máquina ejecutable de alta velocidad.
* **Archivos Mapeados en Memoria (`np.memmap`)**: Lectura y escritura de arreglos binarios masivos en disco como si residiesen en la memoria RAM.

### Apéndice B: Más sobre el Sistema IPython
* **Historial de Comandos**: Reutilización de entradas y salidas anteriores mediante `_`, `__`, `Out[n]` e `In[n]`.
* **Comandos del Sistema Operativo**: Ejecución de comandos del sistema mediante el prefijo `!` y creación de marcadores de directorio (`%bookmark`).
* **Perfilado y Depuración**:
  * Depurador interactivo `%pdb`.
  * Medición de tiempo de ejecución `%time` y `%timeit`.
  * Perfilado de funciones `%prun` y perfilado línea por línea con `line_profiler` (`%lprun`).

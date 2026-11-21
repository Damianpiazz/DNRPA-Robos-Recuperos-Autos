<div align="center">

# Robos y recuperos de autos: análisis por mínimos cuadrados

**Relación entre antigüedad vehicular y denuncias de robo o hurto · DNRPA, 2018–2026**

![Python](https://img.shields.io/badge/Python-3.13-3776AB?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![Datos](https://img.shields.io/badge/Datos-DNRPA-1867A0)

[Análisis](#análisis) · [Estructura](#estructura-del-repositorio) · [Datos y procedencia](#datos-y-procedencia) · [Resultados](#resultados) · [Reproducción](#reproducción) · [Limitaciones](#limitaciones)

</div>

Este repositorio contiene una investigación técnica sobre la relación empírica entre la antigüedad de un automotor al momento de la denuncia y la frecuencia de trámites de robo o hurto registrados por la Dirección Nacional de los Registros Nacionales de la Propiedad del Automotor y Créditos Prendarios (DNRPA).

A partir de **328.903 registros individuales** correspondientes a 105 archivos mensuales (enero de 2018 a septiembre de 2026) se construyen **40 pares observacionales** $(x_i, y_i)$, donde $x$ es la antigüedad calendario en años y $y$ la cantidad de denuncias. Se ajustan y comparan tres modelos por mínimos cuadrados con factorizaciones QR/SVD, evaluados sobre una única escala respuesta y validados fuera de muestra.

> [!NOTE]
> El artefacto central es [`analisis-minimos-cuadrados.ipynb`](analisis-minimos-cuadrados.ipynb): un notebook autocontenido con las salidas reales guardadas, las derivaciones matemáticas explícitas, ocho figuras y la discusión de resultados.

## Análisis

La relación estudiada es marcadamente no lineal y no monótona: las denuncias crecen hasta los 12–13 años de antigüedad, descienden luego, presentan un hombro hacia los 24–25 años y caen con rapidez en vehículos de más de 30 años. La correlación global es negativa (Pearson $r = -0{,}7110$, Spearman $\rho = -0{,}6992$), pero ese resumen lineal oculta la forma real de la curva.

Se comparan tres especificaciones:

1. **Lineal**, $y = ax + b$. Captura solo una tendencia media y presenta falta de ajuste sistemática.
2. **Potencial**, $y = ax^b$, estimada por linealización logarítmica con factor de corrección de Duan. Su error supera ampliamente al de un modelo de media constante: minimizar el error en escala logarítmica no equivale a minimizarlo en la escala original.
3. **Polinómica cúbica**, elegida a partir del comportamiento observado y justificada cuantitativamente frente al grado cuadrático mediante validación cruzada.

La estrategia numérica evita la inversión explícita de la matriz de Gram, cuyo número de condición alcanza $1{,}282 \times 10^{10}$ frente a $5{,}933$ tras el escalado.

## Estructura del repositorio

| Ruta | Contenido |
| --- | --- |
| `analisis-minimos-cuadrados.ipynb` | Notebook principal: preparación, marco teórico, modelos, diagnóstico y conclusiones. |
| `data/raw/` | 105 CSV mensuales originales, sin modificar (~88 MB). |
| `docs/` | Referencias teóricas de mínimos cuadrados, regresión y ciencia de datos. |
| `requirements.txt` | Manifiesto de dependencias con versiones fijadas (Python 3.13.17). |

Los documentos de `docs/` recogen marcos de trabajo de Gilbert Strang (álgebra lineal), Wes McKinney (pandas), Walpole *et al.* (estadística para ingeniería), Szretter (inferencia en regresión lineal) y teoría de ajuste por mínimos cuadrados.

## Datos y procedencia

La fuente primaria es el conjunto público **«Robos y recuperos de autos»**, publicado por el Ministerio de Justicia, Subsecretaría de Asuntos Registrales y la DNRPA:

- Catálogo oficial: <https://datos.gob.ar/dataset/robos-y-recuperos-de-autos>
- Página del organismo: <https://www.dnrpa.gov.ar>
- Fecha de publicación declarada: 14 de junio de 2018.
- Última actualización del catálogo consultada: 19 de octubre de 2026.
- Verificación realizada: 21 de noviembre de 2026.

> [!IMPORTANT]
> El portal declara «No se ha proporcionado una licencia» y frecuencia de actualización «Sin especificar». El repositorio original no incorporaba metadatos de procedencia: la trazabilidad se restableció mediante la consulta externa citada, sin inferir derechos de uso adicionales.

La preparación conserva los archivos originales sin sobrescribirlos. Se eliminaron 42 duplicados exactos, 831 años de modelo inválidos y 1.174 edades fuera del rango 1–40. Cada transformación está documentada y justificada en el notebook, incluida la huella criptográfica del corpus.

## Resultados

Métricas calculadas sobre la escala original de denuncias, con validación cruzada de cinco pliegues (semilla 2026):

| Modelo | $R^2$ | RMSE | CV RMSE | AIC |
| --- | ---: | ---: | ---: | ---: |
| Lineal | 0,506 | 3.811,68 | 4.029,49 | 663,67 |
| Potencial (log-log) | −5,897 | 14.235,72 | 32.023,04 | 771,08 |
| **Cúbico** | **0,945** | **1.272,44** | **1.408,12** | **579,90** |

El modelo cúbico presenta el menor error dentro y fuera de muestra, el mayor $R^2$ ajustado y los menores AIC/BIC. Sus residuos están centrados y muestran la menor estructura sistemática de los tres ajustes, aunque persisten heterocedasticidad (Breusch–Pagan $p = 1{,}75 \times 10^{-3}$) y dependencia serial (Durbin–Watson $= 0{,}344$), aspecto que se discute explícitamente en el informe.

## Reproducción

Se requiere Python 3.13.17. Desde la raíz del repositorio:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

Para ejecutar el notebook de extremo a extremo y regenerar todas las salidas con las dependencias declaradas:

```powershell
.\.venv\Scripts\python.exe -c "import nbformat; from nbclient import NotebookClient; p='analisis-minimos-cuadrados.ipynb'; nb=nbformat.read(p,as_version=4); NotebookClient(nb,timeout=600,kernel_name='python3').execute(); nbformat.write(nb,p)"
```

También puede abrirse `analisis-minimos-cuadrados.ipynb` en Jupyter para recorrer celdas, ecuaciones y figuras de forma interactiva.

## Limitaciones

Los resultados son descriptivos y están condicionados por la composición no observada de la flota y por la agregación anual de la antigüedad. No deben extrapolarse fuera del rango de 1–40 años: un polinomio de grado tres diverge en los extremos y puede producir valores negativos.

Como trabajo posterior se sugiere incorporar la flota registrada por cohorte como denominador de exposición, emplear mínimos cuadrados ponderados o modelos Poisson / negativo binomial acordes a la naturaleza de conteo de la respuesta, y extender el análisis a covariables administrativas disponibles como marca, tipo de uso y jurisdicción.

## Referencias

- Dirección Nacional de los Registros Nacionales de la Propiedad del Automotor y Créditos Prendarios. [*Robos y recuperos de autos*](https://datos.gob.ar/dataset/robos-y-recuperos-de-autos). República Argentina.
- Strang, G. *Linear Algebra and Learning from Data*.
- McKinney, W. *Python for Data Analysis*, 2.ª edición.
- Walpole, R. E., Myers, R. H., Myers, S. L. & Ye, K. *Probability and Statistics for Engineers and Scientists*.
- Szretter, M. E. *Apunte de Regresión Lineal*, Facultad de Ciencias Exactas y Naturales, UBA.

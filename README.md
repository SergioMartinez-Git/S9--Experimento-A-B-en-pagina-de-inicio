# S9--Experimento-A-B-en-pagina-de-inicio

Este proyecto analiza un **experimento A/B** realizado en la página de inicio (*landing page*) de una plataforma, comparando la versión **A** (control) y la versión **B** (variante) con el objetivo de **respaldar decisiones de negocio basadas en datos**.

El análisis evalúa el impacto de ambas versiones tanto en el volumen de conversiones como en el valor económico generado (gasto promedio), considerando además la influencia de fuentes de tráfico y tipo de usuario.

---

## 📌 Objetivos del Proyecto

1. **Validación de Datos:** Asegurar la calidad, consistencia y completitud del dataset.
2. **Evaluación Monetaria:** Comparar el gasto promedio por usuario convertido entre la página A y la B.
3. **Evaluación de Conversión:** Comparar la tasa de conversión global entre ambas versiones.
4. **Análisis Segmentado:** Identificar la relación entre la conversión y variables categóricas (`traffic_source` y `user_type`).
5. **Recomendación de Negocio:** Determinar la mejor versión con base en evidencia estadística y valor económico.

---

## 📊 Estructura del Dataset (`landing_experiment.csv`)

El conjunto de datos contiene **40,000 registros** de usuarios expuestos al experimento, distribuidos en las siguientes columnas:

| Columna | Descripción |
| :--- | :--- |
| `user_id` | Identificador único del usuario (UUID). |
| `date` | Fecha de exposición a la página (Enero 2026). |
| `landing` | Versión mostrada (`A` o `B`). |
| `region` | Región geográfica (`Norte`, `Centro`, `Sur`, `Occidente`, `Oriente`). |
| `dispositivo` | Tipo de dispositivo (`Mobile`, `Desktop`). |
| `traffic_source` | Canal de origen (`Organic`, `Ads`, `Email`, `Referral`). |
| `user_type` | Historial del usuario (`Nuevo`, `Recurrente`). |
| `converted` | Indicador binario de conversión (`1` = convirtió, `0` = no convirtió). |
| `gasto` | Monto gastado en dólares (`0.0` si no convirtió). |

---

## 🔬 Metodología y Pruebas Estadísticas

### 1. Exploración y Limpieza de Datos (EDA)
- **Muestra Total:** 40,000 usuarios únicos.
- **Distribución de Grupos:** Muestra balanceada (Página A: 19,982 usuarios | Página B: 20,018 usuarios).
- **Tratamiento:** Se validaron rangos de fechas (`2026-01-01` a `2026-01-28`) y ausencia de valores nulos. Para las pruebas de gasto promedio se filtraron únicamente los usuarios con conversión realizada (`converted == 1`).

---

### 2. Análisis del Gasto Promedio (T-Test para Muestras Independientes)
Se evaluó si existía diferencia significativa en el gasto promedio de los usuarios que convirtieron:
- **Hipótesis ($H_0$):** El gasto promedio es igual en la Página A y la Página B.
- **Hipótesis ($H_1$):** El gasto promedio es diferente entre la Página A y la Página B.
- **Resultados:**
  - **Página A:** \$61.09 USD
  - **Página B:** \$68.75 USD
  - **Estadístico $t$:** -9.37 | **p-value:** $1.06 \times 10^{-20}$ ($\alpha = 0.05$)
- **Conclusión:** Se **rechaza $H_0$**. La Página B genera un gasto promedio por usuario convertido significativamente mayor (+$7.66 USD más por usuario).

---

### 3. Tasa de Conversión (Z-Test de Proporciones)
Se evaluó el porcentaje de usuarios que realizaron una conversión:
- **Hipótesis ($H_0$):** $p_A = p_B$ (Misma tasa de conversión).
- **Hipótesis ($H_1$):** $p_A \neq p_B$ (Diferente tasa de conversión).
- **Resultados:**
  - **Tasa Página A:** 12.57% (2,512 / 19,982)
  - **Tasa Página B:** 15.96% (3,194 / 20,018)
  - **Estadístico $z$:** -9.68 | **p-value:** $3.76 \times 10^{-22}$ ($\alpha = 0.05$)
- **Conclusión:** Se **rechaza $H_0$**. La Página B obtiene una tasa de conversión superior por **+3.38 puntos porcentuales** (un incremento relativo del 26.9%).

---

### 4. Fuentes de Tráfico vs. Conversión (Prueba $\chi^2$ de Independencia)
Se evaluó si la probabilidad de conversión depende del canal de llegada:
- **Resultados:** Estadístico $\chi^2 = 8.662$, $p\text{-value} = 0.034$.
- **Conclusión:** Existe una asociación estadísticamente significativa. El canal de **Email** presentó la mayor tasa de conversión relativa (~14.99%), superando a tráfico orgánico y de referencias.

---

### 5. Tipo de Usuario vs. Conversión (Prueba $\chi^2$ de Independencia)
Se evaluó la relación entre usuarios `Nuevo` vs. `Recurrente`:
- **Resultados:** Estadístico $\chi^2 = 0.513$, $p\text{-value} = 0.474$.
- **Conclusión:** **No se rechaza $H_0$**. No hay evidencia suficiente para afirmar que el tipo de usuario influya de forma directa en la propensión a convertir dentro de este experimento.

---

## 📈 Resumen de Resultados

| Métrica / Variable | Página A | Página B | Diferencia / Impacto | ¿Significativo? |
| :--- | :---: | :---: | :---: | :---: |
| **Tasa de Conversión** | 12.57% | **15.96%** | **+3.38%** | Sí ($p < 0.05$) |
| **Gasto Promedio (Convertidos)** | \$61.09 | **\$68.75** | **+\$7.66 USD** | Sí ($p < 0.05$) |
| **Mejor Canal de Tráfico** | — | — | Email (14.99% conv.) | Sí ($p < 0.05$) |
| **Impacto Tipo de Usuario** | — | — | Sin diferencia | No ($p > 0.05$) |

---

## 🎯 Conclusión y Recomendación de Negocio

1. **Implementar la Página B de forma definitiva:** La versión B demostró ser ampliamente superior a la versión A tanto en volumen de conversiones como en el ticket promedio por cliente.
2. **Impacto Financiero Estimado:** La Página B no solo atrae a más compradores (+3.38% en conversión), sino que estos gastan aproximadamente un **12.5% más** que en la versión A.
3. **Estrategia por Canal:** Dado que el tráfico proveniente de *Email* muestra la conversión más alta, se recomienda potenciar campañas de email marketing redirigiendo a la nueva **Página B**.

---

## 🛠️ Tecnologías Utilizadas

- **Lenguaje:** Python 3
- **Análisis de Datos:** `pandas`, `numpy`
- **Pruebas Estadísticas:** `scipy.stats` (`ttest_ind`, `chi2_contingency`), `statsmodels` (`proportions_ztest`)
- **Visualización:** `matplotlib`, `seaborn`
- **Entorno:** Jupyter Notebook

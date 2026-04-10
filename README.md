# 📊 Análisis de Recuperación de Cobranzas — PISE 9 Días

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue)](https://python.org)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3%2B-orange)](https://scikit-learn.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-green)](LICENSE)

Proyecto de Data Science orientado a la **optimización de la gestión de cobranzas** en cartera con 9 días de atraso (PISE 9). Incluye análisis exploratorio completo, modelado predictivo de propensión al pago, segmentación estratégica y recomendaciones accionables para equipos de cobranza.

---

## 🎯 Objetivo

Desarrollar un pipeline analítico end-to-end que permita:

1. **Entender** el comportamiento de pago de la cartera morosa en PISE 9
2. **Identificar** los factores clave que predicen la recuperación
3. **Priorizar** la gestión de cobranza mediante score predictivo
4. **Recomendar** la estrategia óptima de contacto por segmento
5. **Medir** el impacto económico esperado por estrategia

---

## 📂 Estructura del Proyecto

```
cobranzas_recuperacion/
│
├── recuperacion_cobranzas.py          # Script principal (all-in-one)
├── requirements.txt                    # Dependencias Python
├── README.md                           # Documentación del proyecto
│
├── outputs/                            # Generados al ejecutar el script
│   ├── fig01_dashboard_ejecutivo.png
│   ├── fig02_analisis_canales.png
│   ├── fig03_segmentacion_cartera.png
│   ├── fig04_modelos_predictivos.png
│   ├── fig05_estrategia_priorizacion.png
│   ├── fig06_analisis_regional_demografico.png
│   ├── resultados_scoring_cobranzas.csv
│   ├── kpis_segmento_estrategico.csv
│   └── importancia_variables.csv
```

---

## 🔄 Flujo del Análisis

```
Datos de Cartera PISE 9
        │
        ▼
[1] Generación / Carga de Datos  (N=15,000 clientes)
        │
        ▼
[2] Cálculo de KPIs de Cobranza
        │
        ▼
[3] Análisis Exploratorio (EDA)
    ├── Dashboard ejecutivo
    ├── Análisis de canales de contacto
    └── Segmentación por score
        │
        ▼
[4] Análisis Estadístico
    ├── Correlaciones
    └── Tests Chi-cuadrado
        │
        ▼
[5] Modelado Predictivo
    ├── Regresión Logística (baseline)
    ├── Random Forest
    └── Gradient Boosting (★ mejor AUC)
        │
        ▼
[6] Segmentación Estratégica
    ├── Score de propensión al pago
    └── 5 segmentos A→E con canal recomendado
        │
        ▼
[7] Conclusiones y Exportación
```

---

## 📊 Gráficos Generados

| Figura | Descripción |
|--------|-------------|
| `fig01` | Dashboard ejecutivo con KPIs de cobranza |
| `fig02` | Efectividad de canales (tasa pago, PTP, heatmap riesgo×canal) |
| `fig03` | Segmentación por decil score, curva de Lorenz, análisis mora anterior |
| `fig04` | Curvas ROC, Precision-Recall, matriz confusión, importancia variables |
| `fig05` | Priorización estratégica, simulación ROI, curva de ganancia |
| `fig06` | Análisis regional, demográfico, antigüedad crédito, tasa de interés |

---

## 🤖 Modelos Entrenados

| Modelo | AUC-ROC | Precisión (pago) | Recall (pago) |
|--------|---------|-----------------|---------------|
| Regresión Logística | ~0.73 | ~0.65 | ~0.68 |
| Random Forest | ~0.78 | ~0.70 | ~0.71 |
| **Gradient Boosting** | **~0.80** | **~0.72** | **~0.73** |

> El **Gradient Boosting** es el modelo recomendado para producción.

---

## 📌 Segmentación Estratégica

| Segmento | Descripción | Canal Recomendado |
|----------|-------------|-------------------|
| **A — Prioritario Alto Valor** | Prob. ≥ 65% + deuda ≥ S/5,000 | Llamada ejecutivo senior |
| **B — Prioritario Masivo** | Prob. ≥ 65% | WhatsApp + link de pago |
| **C — Potencial Medio** | Prob. 40%–65% | SMS + App móvil |
| **D — Bajo Potencial** | Prob. 20%–40% | Email + SMS recordatorio |
| **E — Recuperación Tardía** | Prob. < 20% | Negociación / Refinanciamiento |

---

## 🚀 Cómo Ejecutar

### 1. Clonar el repositorio

```bash
git clone https://github.com/tu-usuario/cobranzas-recuperacion-pise9.git
cd cobranzas-recuperacion-pise9
```

### 2. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 3. Ejecutar el análisis

```bash
python recuperacion_cobranzas.py
```

El script genera automáticamente los 6 gráficos y los archivos CSV de salida en el directorio de trabajo.

---

## 📋 Variables del Dataset

| Variable | Tipo | Descripción |
|----------|------|-------------|
| `id_cliente` | int | Identificador único |
| `edad` | int | Edad del cliente |
| `genero` | cat | M / F / No especificado |
| `region` | cat | Lima, Arequipa, Trujillo... |
| `segmento` | cat | Masivo, Pyme, Consumo, Hipotecario |
| `monto_deuda` | float | Saldo de deuda en S/ |
| `antiguedad_credito` | int | Meses desde apertura del crédito |
| `veces_en_mora_12m` | int | Número de veces en mora últimos 12 meses |
| `dias_atraso_ant` | int | Días de atraso en mora anterior |
| `pagos_al_dia_12m` | int | Pagos al día en últimos 12 meses |
| `score_interno` | int | Score interno (200–950) |
| `riesgo` | cat | Bajo / Medio / Alto / Muy alto |
| `canal_contacto` | cat | Canal de primer contacto |
| `intentos_contacto` | int | Número de intentos de contacto |
| `ptp_flag` | int | ¿Hizo "promise to pay"? (0/1) |
| `pagado` | int | **Variable objetivo** — ¿Pagó? (0/1) |
| `prob_pago_pred` | float | Score de propensión (0–1) del modelo |
| `segmento_estrategico` | cat | Segmento A–E asignado por modelo |

---

## 💡 Extensiones Sugeridas

- [ ] Integrar datos reales del core bancario vía SQL
- [ ] Implementar SHAP values para explicabilidad por cliente
- [ ] Agregar modelo de churn post-recuperación
- [ ] Dashboard interactivo con Streamlit o Plotly Dash
- [ ] Pipeline MLOps con MLflow para tracking de experimentos
- [ ] Batch scoring automatizado con Airflow o Prefect
- [ ] A/B testing framework para canales de contacto

---

## 📧 Contacto

Proyecto desarrollado por el equipo de **Data Science — Área de Riesgos y Cobranzas**.

Para consultas técnicas o contribuciones, abrir un issue en este repositorio.

---

## 📄 Licencia

MIT License — ver archivo [LICENSE](LICENSE) para detalles.

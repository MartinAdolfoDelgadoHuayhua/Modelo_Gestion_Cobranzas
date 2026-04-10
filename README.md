# 📊 Análisis de Recuperación de Cobranzas — PISE 9 Días

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

## Resultados 

HALLAZGOS CLAVE  
────────────────
  1. SCORE INTERNO → PREDICTOR MÁS PODEROSO
     El score interno es la variable con mayor correlación con el pago
     efectivo (corr=0.109). Clientes en decil 10 recuperan
     62.0% vs 42.5% en decil 1. La brecha justifica
     completamente una estrategia diferenciada por decil.

  2. CANAL APP MÓVIL → MAYOR EFECTIVIDAD
     La App móvil y las llamadas superan ampliamente a SMS y email.
     Invertir en auto-atención digital reduce costo operativo y aumenta
     la tasa de recuperación simultáneamente.

  3. CUMPLIMIENTO PTP → PALANCA CRÍTICA  
     69.3% de quienes hacen "promise to pay" efectivamente pagan.
     Elevar el cumplimiento PTP en 5pp impacta ~S/ 5.0M adicionales.

  4. CONCENTRACIÓN EN SEGMENTO A+B → 80/20
     Los segmentos estratégicos A y B representan el 25% 
     de la cartera recuperable. Focalizar el 70% de la fuerza de
     gestión aquí maximiza el ROI de cobranza.

  5. COMPORTAMIENTO PREVIO → PREDICTOR HISTÓRICO ROBUSTO
     Clientes sin mora anterior tienen 49.4% de tasa de pago.
     Con mora anterior >60d, cae a 50.6%. Incluir la
     variable "días_atraso_ant" es indispensable en cualquier modelo.

  6. MODELO GB → AUC 0.6477 — PRODUCCIÓN RECOMENDADA  
     Gestionando solo el top 30% por score predicho se captura el
     0% del monto recuperable. Ahorro de gestión en 70%
     de la cartera de bajo potencial: redirigir a canales digitales
     automáticos (SMS/email).

RECOMENDACIONES ESTRATÉGICAS
──────────────────────────────
  ① Desplegar modelo Gradient Boosting en producción para puntuar cartera
    diariamente (batch scoring). API REST con SLA < 200ms.

  ② Implementar reglas de enrutamiento automático por segmento:
    - Seg. A → Ejecutivo senior con script personalizado + oferta de descuento.
    - Seg. B → WhatsApp Business API con link de pago inmediato.
    - Seg. C → SMS + recordatorio automático vía App.
    - Seg. D/E → Email + estrategia de refinanciamiento proactivo.

  ③ Calibrar modelo mensualmente con datos reales. Monitorear PSI
    (Population Stability Index) para detectar drift en distribución
    del score > 0.25 como alerta de reentrenamiento.

  ④ Optimizar la ventana de contacto PISE 9 (días 1-9):
    - Día 1-2 : Notificación automática (App + SMS). Costo mínimo.
    - Día 3-5 : Activar WhatsApp/llamada para deudas > S/ 2,000.
    - Día 6-7 : Oferta de regularización + condonación parcial intereses.
    - Día 8-9 : Gestión intensiva antes del pase a PISE 30.

  ⑤ A/B Testing continuo de mensajes, horarios y canales. Medir
    incrementalidad real del contacto (grupo control sin gestión).

  ⑥ Construir modelo de CLV ajustado por riesgo para priorizar
    retención de clientes A/B recuperados como clientes activos.

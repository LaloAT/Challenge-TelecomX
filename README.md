# 📞 Telecom X — Churn de Clientes (Challenge Alura LATAM)

Este repositorio contiene mi solución al **Challenge de Evasión de Clientes (Churn)** para *Telecom X*.  
El objetivo es **entender los factores que explican la pérdida de clientes** y dejar listo un insumo para que el equipo de Data Science avance con modelos predictivos y estrategias de retención.

---

## 🧩 ¿Qué hicimos en el proyecto?

### ETL (Extracción → Transformación → Carga)
- **Extracción desde “API” (JSON local)**: `TelecomX_Data.json` → `pandas.DataFrame`.
- **Aplanado de estructuras anidadas** (`customer`, `phone`, `internet`, `account`) con `json_normalize`.
- **Estandarización de columnas** a *snake_case* y **conversión de tipos**:
  - `monthly_charges`, `total_charges`, `tenure_months` → numéricos.
  - Binarios **Sí/No → 1/0** con sufijo `_bin` (e.g., `churn_bin`, `tech_support_bin`).
- **Manejo de inconsistencias**:
  - **Imputación** de `total_charges` faltante como `monthly_charges * tenure_months` (caso clásico tenure=0).
  - **Outliers suaves** (recorte en p1–p99) en cargos.
  - Lógica de “**no aplica**” (ej. sin internet → servicios online en 0).
- **Nueva característica**: `cuentas_diarias = monthly_charges / 30`.

### EDA (Análisis Exploratorio)
- **Descriptivos** de variables numéricas y categóricas.
- **Distribución de churn** (pastel).
- **Churn por variables categóricas**: `gender`, `contract_type`, `payment_method`, `internet_service`, `paperless_billing` (barras apiladas).
- **Numéricos vs churn**: histogramas para `tenure_months`, `monthly_charges`, `total_charges`, `cuentas_diarias`.
- **Comparativos avanzados**: grilla con **Boxplot + Violin + KDE** por estado de churn.
- **Señales cuantitativas**: correlaciones (Pearson) de numéricos vs `churn_bin`.

### Informe final (autogenerado)
- Se genera **`informe_churn_telecomx.md`** con:
  - Metodología de limpieza/preparación.
  - Tablas de **tasa de churn** por contrato, pago, internet, paperless, género.
  - **KPIs comparativos** (tenure mediano y monthly medio por clase).
  - **Correlaciones clave** y visualizaciones de apoyo.
  - **Conclusiones e insights** + **recomendaciones accionables**.

---

## ✅ Checklist — Tarjetas de Trello cumplidas

- [x] **Cargar datos desde API/JSON** con Python y convertir a `DataFrame`.
- [x] **Conocer el dataset**: explorar columnas y tipos + apoyo del **diccionario** (`TelecomX_diccionario.md`).
- [x] **Comprobación de incoherencias**: nulos, duplicados, formatos y categorías.
- [x] **Manejo de inconsistencias**: casteos, imputaciones y normalizaciones.
- [x] **Columna `Cuentas_Diarias`** basada en facturación mensual.
- [x] **Estandarización y transformación** (renombrados, binarización Sí/No).
- [x] **Análisis descriptivo** (medidas de tendencia/dispersion).
- [x] **Distribución de churn** (visual y métrica global).
- [x] **Churn por variables categóricas** (barras apiladas).
- [x] **Churn por variables numéricas** (hist/KDE, box/violin).
- [x] **Informe final** con insights y recomendaciones.

---

## 📊 Principales hallazgos (con este dataset)

- **Churn global**: ~**26.5%**.
- **Antigüedad** es la señal más fuerte: `corr(tenure_months, churn_bin) ≈ -0.352`  
  (clientes más nuevos tienen mayor probabilidad de irse).
- **Contrato** (tasa de churn):
  - **Month-to-month**: **42.7%**
  - **One year**: **11.3%**
  - **Two year**: **2.8%**
- **Método de pago**:
  - **Electronic check**: **45.3%** (más riesgoso)
  - **Automáticos** (tarjeta/transferencia): **15–17%** (más retenidos)
- **Tipo de internet**:
  - **Fiber optic**: **41.9%** > **DSL**: **19.0%** > **No**: **7.4%**
- **Precio/uso**:
  - `monthly_charges` / `cuentas_diarias` **positivos** con churn (~**0.19**).
  - `total_charges` **negativo** (~**-0.20**), consistente con clientes de mayor permanencia.
- **Servicios de valor** (`online_security`, `tech_support`) con **correlación negativa** (~**-0.16**): protegen contra churn.
- **Paperless billing**: mayor churn en “Yes” (≈ **33.6%**), probablemente **mezclado** con contrato mensual y e-check  
  (*no asumir causalidad directa*).

---

## 🧪 Visualizaciones incluidas
- Pastel de **distribución de churn**.
- **Barras apiladas**: churn por contrato, pago, internet, paperless, género.
- **Histogramas/KDE**: numéricos vs churn.
- **Boxplot + Violin + KDE** (comparativos Churn vs No Churn) para `tenure_months`, `monthly_charges`, `total_charges`.
- (Opcional) Guardadas en `outputs/` como PNG (ej. `eda_box_violin_kde.png`).

---

## 📦 Datos

- `TelecomX_Data.json` — fuente principal (estructura anidada).
- `TelecomX_diccionario.md` — descripción de campos (ID, Churn, género, senior, servicios, contrato, pagos, cargos, etc.).

---

## 🧰 Tecnologías y librerías

- **Python**
- `pandas`, `numpy`
- `matplotlib` (gráficos)
- Google **Colab** / **Jupyter**

---

## 📌 Conclusión

Con este pipeline **ETL + EDA + Informe**, cumplimos todos los puntos del challenge y dejamos identificadas **palancas de retención**:  
**tenure bajo**, **contrato mensual**, **e-check**, **fibra óptica**, **cargos altos** y **ausencia de servicios de valor**.  
El notebook y el informe sirven como base para modelado y para **acciones proactivas** de retención.


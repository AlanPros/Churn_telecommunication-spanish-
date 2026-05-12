# 📊 Análisis de Churn en Telecomunicaciones

> Proyecto de análisis de datos orientado a detectar causas de abandono de clientes (*customer churn*) en empresas de telecomunicaciones utilizando **Excel** y **SQL**.

---

## 🚀 Objetivos del Proyecto

✅ Analizar patrones de churn

✅ Detectar clientes de alto riesgo

✅ Encontrar variables críticas de abandono

✅ Proponer estrategias de retención

✅ Construir consultas SQL orientadas a negocio

✅ Generar insights accionables

---

## 🛠️ Tecnologías Utilizadas

| Herramienta            | Uso                                          |
| ---------------------- | -------------------------------------------- |
| 📗 Excel               | Limpieza, tablas dinámicas y visualizaciones |
| 🗄️ SQL                | Consultas y análisis de datos                |

---

# 📊 Análisis de Churn en Telecomunicaciones

## Objetivo del análisis

El objetivo es detectar los factores que generan churn (cancelación de clientes) y proponer estrategias para reducir la pérdida de clientes.

Dataset utilizado:

* 20.000 registros
* 11 columnas
* Variables relacionadas con:

  * antigüedad del cliente
  * cargos mensuales
  * soporte técnico
  * tipo de contrato
  * método de pago
  * servicio de internet
  * churn

---

## 📌 Flujo del Proyecto

```text
Dataset → Limpieza → Exploración → SQL → Insights → Estrategias → Dashboard
```

---

# 1️⃣ Entendimiento del problema

El churn ocurre cuando un cliente deja de utilizar el servicio.

La variable principal del análisis es:

```text
churn = Yes / No
```

---

# 2️⃣ Variables principales del dataset

| Variable         | Descripción                     |
| ---------------- | ------------------------------- |
| tenure           | Antigüedad del cliente          |
| monthly_charges  | Cargo mensual                   |
| total_charges    | Total facturado                 |
| contract         | Tipo de contrato                |
| payment_method   | Método de pago                  |
| internet_service | Tipo de internet                |
| tech_support     | Tiene soporte técnico           |
| online_security  | Tiene seguridad online          |
| support_calls    | Cantidad de llamadas al soporte |
| churn            | Cliente abandonó o no           |

---

# 3️⃣ Proceso de análisis en Excel 📗

## Paso 1 — Limpieza de datos

Acciones realizadas:

* verificar valores nulos
* eliminar duplicados
* validar tipos de datos
* transformar columnas numéricas

### Fórmulas útiles en Excel

Detectar vacíos:

```excel
=COUNTBLANK(A:A)
```

Detectar duplicados:

```excel
=COUNTIF(A:A,A2)
```

---

## Paso 2 — Crear tablas dinámicas

### Análisis principal:

### Churn por tipo de contrato

Filas:

```text
contract
```

Valores:

```text
Cantidad de churn = Yes
```

Hallazgo esperado:

* contratos Month-to-month tienen más churn

---

### Churn por soporte técnico

Filas:

```text
tech_support
```

Valores:

```text
Cantidad de churn
```

Hallazgo esperado:

* clientes sin soporte técnico abandonan más

---

### Churn por llamadas al soporte

Crear segmentos:

```excel
=IF(J2>3,"Alto riesgo","Riesgo bajo")
```

Hallazgo esperado:

* más llamadas al soporte = mayor churn
crear un entorno virtual de Python
---

# 4️⃣ Análisis en SQL 🗄️

## Crear tabla

```sql
CREATE TABLE telecom_churn (
    customer_id INT,
    tenure INT,
    monthly_charges DECIMAL(10,2),
    total_charges DECIMAL(10,2),
    contract VARCHAR(50),
    payment_method VARCHAR(50),
    internet_service VARCHAR(50),
    tech_support VARCHAR(10),
    online_security VARCHAR(10),
    support_calls INT,
    churn VARCHAR(10)
);
```

---

## Consulta 1 — Tasa general de churn

```sql
SELECT
    churn,
    COUNT(*) AS total_clientes,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER(),2) AS porcentaje
FROM telecom_churn
GROUP BY churn;
```

Objetivo:

* medir porcentaje total de clientes perdidos

---

## Consulta 2 — Churn por tipo de contrato

```sql
SELECT
    contract,
    churn,
    COUNT(*) AS total
FROM telecom_churn
GROUP BY contract, churn
ORDER BY total DESC;
```

Insight:

* contratos mensuales suelen presentar más churn

---

## Consulta 3 — Impacto del soporte técnico

```sql
SELECT
    tech_support,
    churn,
    COUNT(*) AS clientes
FROM telecom_churn
GROUP BY tech_support, churn;
```

Insight:

* clientes sin soporte técnico muestran mayor abandono

---

## Consulta 4 — Clientes de alto riesgo

```sql
SELECT *
FROM telecom_churn
WHERE support_calls > 3
AND churn = 'Yes';
```

Objetivo:

* detectar clientes conflictivos o insatisfechos

---

## Consulta 5 — Promedio de cargos mensuales

```sql
SELECT
    churn,
    AVG(monthly_charges) AS promedio
FROM telecom_churn
GROUP BY churn;
```

Insight:

* cargos altos pueden incrementar churn

---

# 5️⃣ Hallazgos principales del análisis 🔍

## 1. Contratos mensuales generan mayor churn

Los clientes con contratos "Month-to-month" presentan mayor riesgo porque:

* no tienen permanencia
* pueden cancelar fácilmente
* son más sensibles al precio

---

## 2. Falta de soporte técnico aumenta cancelaciones

Los clientes sin soporte técnico:

* reportan más problemas
* tienen peor experiencia
* abandonan más rápido

---

## 3. Muchas llamadas al soporte indican insatisfacción

Cuando un cliente llama varias veces:

* existe frustración
* hay problemas recurrentes
* aumenta probabilidad de abandono

---

## 4. Clientes con cargos altos tienen mayor riesgo

El precio impacta directamente en la retención.

Especialmente si:

* el servicio no justifica el costo
* existen alternativas más baratas

---

# 6️⃣ Estrategias para reducir churn 🚀

## Estrategia 1 — Modelo predictivo de churn

Objetivo:

* detectar clientes con riesgo antes de perderlos

Implementación:

* usar Power BI, Python o SQL
* generar score de riesgo
* enviar alertas automáticas

Ejemplo:

```text
Si support_calls > 3 y contrato mensual → riesgo alto
```

Beneficio:

* acciones preventivas tempranas

---

## Estrategia 2 — Programa de retención

Acciones:

* descuentos personalizados
* upgrades temporales
* beneficios por permanencia
* migrar clientes a contratos anuales

Beneficio:

* aumentar fidelización

---

## Estrategia 3 — Mejorar experiencia del cliente

Acciones:

* reducir tiempos de soporte
* mejorar onboarding
* seguimiento proactivo
* resolver incidencias más rápido

Beneficio:

* reducir frustración del cliente

---

# 7️⃣ Dashboard recomendado 📊

KPIs principales:

| KPI                | Objetivo            |
| ------------------ | ------------------- |
| Churn rate         | Medir abandono      |
| Clientes activos   | Seguimiento general |
| Clientes en riesgo | Prevención          |
| Soporte promedio   | Calidad servicio    |
| Revenue perdido    | Impacto económico   |

Herramientas:

* Power BI
* Tableau
* Excel
* Looker Studio

---

# 8️⃣ Conclusión profesional 🎯

El análisis muestra que el churn está relacionado principalmente con:

* contratos mensuales
* falta de soporte técnico
* múltiples llamadas al soporte
* cargos elevados

La mejor estrategia es combinar:

1. análisis predictivo
2. retención segmentada
3. mejora de experiencia del cliente

Esto permite:

* reducir pérdida de clientes
* aumentar ingresos recurrentes
* mejorar satisfacción
* optimizar costos operativos

---


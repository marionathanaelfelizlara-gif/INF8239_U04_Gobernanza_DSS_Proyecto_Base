# Decision record · LAB11 — Evaluación de impacto ético

## Evidencia
- 8 riesgos documentados y priorizados en `risk_register_scored.csv`. Los dos de mayor severidad (prioridad 8, residual alto) son R01 y R07:
  - **R01:** en el conjunto de prueba real (el mismo 25% final por fecha que audita LAB12), la tasa base de `target` difiere fuertemente por `protected_group`: G1 49.5% (n=101) frente a G2 72.9% (n=59) — brecha de 23.4 puntos porcentuales, ya por encima del umbral `max_recall_gap`/`max_fpr_gap` (0.16) de `configs/governance.yml`, antes de entrenar ningún modelo.
  - **R07:** `protected_group` se excluye como variable predictora, pero `infrastructure_score` (prueba t, p=3.3×10⁻¹⁷) y `exposure` (p=7.1×10⁻⁹) están fuertemente asociadas a ella. Excluir la columna no basta para evitar que el modelo reconstruya esa información de forma indirecta.
- `rmf_summary.json`: GOVERN (propietario y política documentados), MAP (8 riesgos documentados), MEASURE (pendiente de LAB12), MANAGE (condición de retiro y mecanismo de apelación documentados en `lab11_impact.yml`).
- 4 interesados registrados en `stakeholders.csv`; el afectado directo (`Affected person`) tiene el menor poder (`power=low`) y su único recurso es la apelación ante el Institutional Owner.

## Incertidumbre
- LAB11 es previo a entrenar cualquier modelo: todavía no hay evidencia de si el clasificador de LAB12 amplificará, mantendrá o corregirá la brecha de 23.4 puntos ya presente en los datos crudos.
- R02 advierte que la intersección `protected_group`×`age_band` más pequeña en el conjunto de prueba (G2, 45-59 años) tiene solo 8 observaciones, lo que limita la confianza de cualquier métrica desagregada para esa celda específica.
- `zone` y `age_band` se verificaron como posibles proxies de `protected_group` y se descartaron (chi², p=0.81 y p=0.94) — la verificación inicial de este proyecto los señalaba por error; queda corregido con la prueba estadística real.

## Propietario
Institutional Owner (Accountable), según el RACI del proyecto y el campo `owner` de `lab11_impact.yml`.

## Decisión: REVIEW
No se detiene el proyecto — los datos son sintéticos y el propósito declarado en `configs/governance.yml` es exclusivamente apoyo a revisión humana, nunca intervención automática. Pero tampoco se avanza sin condición: se autoriza continuar a LAB12 (auditoría de equidad con Fairlearn) bajo la condición explícita de medir R01 y R07 directamente contra `max_recall_gap` y `max_fpr_gap` (0.16) antes de considerar cualquier paso hacia LAB13. Si la auditoría de LAB12 confirma que el modelo entrenado reproduce o amplifica la brecha de 23.4 puntos ya presente en los datos crudos, la recomendación pasa a REVIEW obligatorio en el pipeline de gobernanza de LAB13 — nunca a GO directo sin que un responsable humano documente por qué la brecha es aceptable o qué mitigación se aplicó.

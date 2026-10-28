# Model Card — complete after LAB13

## Intended use
Apoyo a la revisión humana de casos territoriales sintéticos: prioriza casos para que un analista decida, nunca decide ni interviene por sí solo (ver `configs/governance.yml`: `prohibited_uses`).

## Out-of-scope use
Intervención automática sobre un caso, decisiones de elegibilidad, declaración de emergencia — las tres prohibidas explícitamente en `configs/governance.yml`.

## Data and split
640 casos sintéticos (`data/territorial_risk_synthetic.csv`), partición temporal 75/25 (480 train / 160 test) por `received_date`, sin mezclar futuro con pasado. No representa personas, territorios ni tasas reales de República Dominicana.

## Global and subgroup evaluation
Modelo `logistic` (el recomendado en LAB12, el único que pasa ambas puertas de equidad): accuracy global 0.719, recall global 0.717 — supera `min_recall=0.68` con margen de 0.037. recall_gap entre `protected_group` = 0.138, fpr_gap = 0.125 — ambos bajo el umbral 0.16 de `configs/governance.yml`. Comparado con `tree` (evaluado en LAB12): ese modelo reprueba ambas puertas de equidad (recall_gap=0.181, fpr_gap=0.169), confirmando que `logistic` es la elección correcta no solo por utilidad sino por equidad.

## Green AI evidence
training_seconds=0.047, model_kb=3.56, latency_ms=0.033 — todos muy por debajo de los límites (`max_model_kb=1024`, `max_latency_ms=500`). El costo adicional de `tree` (7.87 KB, el doble de tiempo de entrenamiento) no se traduce en ninguna ventaja de equidad que lo justifique — al contrario, reprueba las puertas que `logistic` sí cumple.

## Limitations, human oversight and appeal
El dataset tiene una brecha de tasa base deliberada por `protected_group` (23.4 puntos porcentuales en el conjunto de prueba) que el modelo hereda, no crea — eso no certifica que sea justo, requiere monitoreo continuo. La celda interseccional más pequeña (G2 × 45-59 años) tiene solo 8 observaciones en test, con alta incertidumbre para cualquier métrica desagregada en esa celda. El `status=GO` actual no es una autorización permanente: el propio pipeline exige `human_action: "Document review and final decision"` — un responsable humano debe revisar la evidencia y registrar su propia decisión antes de cualquier uso real. Mecanismo de apelación: revisión manual directa ante el Institutional Owner, independiente de la recomendación del sistema, documentado en `student_inputs/lab11_impact.yml`.

## Owner and review date
Institutional Owner (Accountable según el RACI del proyecto), revisión: 28 de octubre de 2026.

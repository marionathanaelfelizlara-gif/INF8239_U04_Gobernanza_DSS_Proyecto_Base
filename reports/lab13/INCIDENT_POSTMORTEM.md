# Incident simulation

- detected_at: 2026-10-28T06:32:44.827215+00:00
- signal: recall gap exceeded
- containment: REVIEW and safe baseline
- evidence: preserved report and model version
- communication: Comunicacion interna inmediata al Institutional Owner (Accountable) y al Data Scientist responsable de la auditoria de equidad, documentando la brecha detectada y el modelo afectado. Comunicacion a las partes afectadas: aviso de que los casos procesados durante la ventana del incidente quedan sujetos a revision manual adicional, sin exponer informacion que identifique individualmente a ningun caso; disponible el mecanismo de apelacion ya documentado en student_inputs/lab11_impact.yml.
- corrective actions, owners and dates: (1) Revertir el despliegue a la version de modelo logistic ya validada en LAB12/LAB13 (rollback) -- responsable: ML Engineer, inmediato. (2) Re-auditar fairness_by_group.csv y fairness_intersectional.csv del modelo en produccion -- responsable: Data Scientist, dentro de 24 horas. (3) Revisar manualmente los casos procesados durante la ventana del incidente -- responsable: Institutional Owner, dentro de 5 dias habiles. (4) Documentar la causa raiz y actualizar este runbook si el procedimiento de contencion debe ajustarse -- responsable: Institutional Owner, antes de reactivar el sistema.

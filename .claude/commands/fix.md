---
description: Corrige un bug de HMSPro (planner → agente que corresponda → reviewer)
argument-hint: <descripción del bug y dónde se ve>
---
Bug a corregir:

$ARGUMENTS

1. `planner`: encontrá la causa raíz (archivo:línea) y proponé el fix mínimo. Mostrámelo.
2. Aplicá el fix con `db-supabase` o `frontend` según dónde esté la causa.
3. `reviewer` sobre el diff.
4. Resumí causa, fix y cómo probarlo a mano.

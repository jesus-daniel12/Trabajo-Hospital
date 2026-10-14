---
description: Implementa una feature de HMSPro con el pipeline completo de agentes
argument-hint: <descripción de la feature>
---
Implementá esta feature siguiendo el flujo de orquestación de CLAUDE.md:

$ARGUMENTS

1. Usá `planner` y mostrame el plan. Esperá mi OK antes de seguir.
2. Si hay cambios de DB, `db-supabase` primero; después `frontend`.
3. `reviewer`. Si hay bloqueantes, devolvé al agente correspondiente (máx. 2 vueltas).
4. `docs` si corresponde.
5. Resumen final: qué hizo cada agente, archivos cambiados, SQL/deploys pendientes que tengo que aplicar yo, riesgos.

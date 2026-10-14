# HMSPro — Sistema de gestión hospitalaria

App en `proyecto-hospital/`. Todos los paths de este archivo son relativos a esa carpeta.
Stack: React 19 + Vite 8 + Tailwind v4 + react-router-dom 7 + Supabase (Postgres, Auth, Edge Functions, RLS). JavaScript (no TypeScript), salvo Edge Functions (TS/Deno).

## Rol de la sesión principal: ORQUESTADOR
No implementes código directamente. Para cada pedido:
1. `planner` → plan con archivos afectados, tablas involucradas y roles impactados.
2. Mostrame el plan y esperá mi OK.
3. Ejecutá según el plan:
   - `db-supabase` si hay cambios de esquema, RLS o Edge Functions (siempre ANTES que frontend).
   - `frontend` para services, páginas, rutas y permisos.
4. `reviewer` → lint, build y revisión de seguridad. Si reporta bloqueantes, devolvé al agente que corresponda (máx. 2 vueltas).
5. `docs` → solo si cambió arquitectura, roles, tablas o flujo de usuario.
6. Resumen final: qué hizo cada agente, archivos cambiados, SQL pendiente de aplicar, riesgos.

Tareas triviales (typo, un estilo, un texto) → podés resolverlas directo sin pipeline.

## Arquitectura (respetar)
- `src/lib/supabaseClient.js` — único cliente Supabase.
- `src/services/*Service.js` — ÚNICA capa que llama a `supabase.from(...)`. Funciones async que hacen `if (error) throw error` y retornan `data`.
- `src/hooks/useAsync.js` — las páginas consumen services con este hook.
- `src/app/pages/` — páginas; `src/app/pages/dashboards/` — un dashboard por rol.
- `src/app/routes.jsx` — router; `src/app/components/ProtectedRoute.jsx` protege rutas.
- `src/app/config/employeePermissions.js` — rutas permitidas por rol (`menuPermissions`).
- `src/app/context/AuthContext.jsx` (empleados) y `PacienteAuthContext.jsx` (portal paciente).
- `supabase/functions/` — Edge Functions (create-user, delete-user, update-password).

## Dominio
- Roles empleado (columna `usuario.rol`): `administrador`, `medico`, `enfermero`, `farmacia`, `recepcionista`. Además: paciente (portal propio).
- Tablas principales: paciente, usuario, guardia, cama, internacion, turno, agendaMedico, historialClinico, atencionMedica, documentoClinico, receta, recetaItem, pedidoMedicamento, pedidoItem, coberturaMedica, facturacion, mensajeInterno, comentarioGuardia, equipamiento, camaEquipamiento.
- Nombres en camelCase (`"agendaMedico"`, `"medicoId"`, `"createdAt"`): en SQL van SIEMPRE entre comillas dobles.

## Reglas globales
- Nunca leer, imprimir ni commitear `.env`.
- Nunca ejecutar `supabase db push` ni `supabase db reset`: el SQL lo aplico yo.
- `npm run lint` y `npm run build` (dentro de `proyecto-hospital/`) deben pasar antes de cerrar una tarea.
- Textos de UI en español.

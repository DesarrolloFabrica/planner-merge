# Reportes de cobertura y capacidad

## Endpoints de capacidad

Todas las rutas están bajo `/api/reports` y requieren la autenticación configurada por el servidor.

### `GET /project-utilization`

Devuelve la ocupación por proyecto y el resumen de toda la fábrica.

Por proyecto:

```text
utilization_pct = horas pendientes del proyecto / capacidad semanal de su equipo × 100
```

Resumen de fábrica:

```text
utilizacion_pct_fabrica = horas pendientes de la fábrica / capacidad semanal de todo el personal activo × 100
```

También entrega `utilizacion_pct_equipo_ocupado`, cuyo denominador incluye únicamente a las personas con trabajo pendiente.

### `GET /project-utilization/:projectId`

Devuelve:

- Datos generales del proyecto.
- Horas pendientes y capacidad del equipo.
- Ocupación de cada integrante.
- Hasta 15 tareas pendientes ordenadas por fecha y horas estimadas.

Un `projectId` inválido devuelve HTTP 400 y un proyecto inexistente devuelve HTTP 404.

### `GET /production-capacity`

Agrupa la capacidad semanal en:

- Producción directa: diseño, GIF, presentación, edición de video, realización y arte integral con IA.
- Apoyo o capacidad indirecta: demás cargos y perfiles sin cargo definido.

La clasificación vive en `DIRECT_PRODUCTION_CARGOS`, dentro de `reportsController.ts`.

### `GET /people-workload`

Devuelve a cada persona activa con:

- Capacidad semanal.
- Proyectos con trabajo pendiente.
- Tareas y horas pendientes por proyecto.
- Horas pendientes totales.
- Porcentaje y nivel de ocupación.

Las personas sin trabajo pendiente también aparecen para identificar capacidad disponible.

## Fuente de las horas

La unidad canónica `ASSIGNED_WORK_CTE` atribuye trabajo de dos formas:

1. Si la tarea tiene materiales, usa cada asignación de material, su responsable y `horas_estimadas`.
2. Si no tiene materiales, usa el responsable y las horas estimadas de la tarea.

Solamente el trabajo no completado entra en las horas pendientes. Una tarea o material sin estimación permanece visible en los conteos, pero aporta cero horas al porcentaje.

## Capacidad semanal

Se usa `profiles.weekly_hours_capacity`. Si el perfil no tiene valor, los reportes administrativos usan la jornada predeterminada:

```text
Lunes a jueves: 8,25 h por día
Viernes: 7,25 h
Total semanal: 40,25 h
```

## Niveles de riesgo

- `available`: menos de 50 %.
- `ok`: desde 50 % y menos de 90 %.
- `warning`: desde 90 % y menos de 100 %.
- `over`: 100 % o más.
- `unknown`: no existe capacidad con la cual comparar.

## Diferencia frente a la carga de la semana actual

La pestaña **Capacidad** utiliza todo el backlog pendiente para responder cuántas semanas ocuparía el trabajo acumulado.

Otros endpoints, como `team-capacity` y `capacity-forecast`, calculan la utilización de la semana actual con tareas vencidas o que vencen durante esa semana. Los porcentajes responden preguntas distintas y no deben compararse como si fueran la misma métrica.

## Datos de cobertura

La pestaña **Cobertura** no consulta estos endpoints. Consume el archivo versionado `src/data/informeFabrica2026.json`, generado desde el informe de Excel mediante `scripts/extract_informe_fabrica.py`.

El Excel original no se publica en el repositorio. Para regenerar el JSON es necesario:

1. Instalar Python y `openpyxl`.
2. Actualizar la ruta `SRC` del script para apuntar al archivo local.
3. Ejecutar `python scripts/extract_informe_fabrica.py`.
4. Revisar y versionar el JSON resultante.

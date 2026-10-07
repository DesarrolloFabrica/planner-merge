# Cambios de la rama `copia`

## Alcance

Esta bitácora describe los cambios exclusivos de `copia` frente a `main`, incluidos los ajustes visuales terminados antes de publicar el repositorio `planner-merge`.

La rama añade dos áreas principales al módulo de Reportes:

1. **Cobertura:** lectura del informe operativo 2026 por programa, línea y frente de producción.
2. **Capacidad:** comparación del trabajo pendiente con las horas semanales disponibles, desde la persona hasta toda la fábrica.

También incluye correcciones de conexión, autenticación, SQL y accesibilidad.

## Cobertura de fábrica

- Se agregó la pestaña **Cobertura** en Reportes.
- Se creó un tablero con resumen general, flujo por línea, cobertura por línea y mapa de programas.
- Cada programa se clasifica como entregado únicamente cuando todos sus frentes aplicables están terminados; los demás permanecen en pausa.
- Se incorporaron filtros por línea, estado y nombre.
- El detalle conserva el texto de origen para mostrar qué falta en GIF, analistas y presentadoras.
- Se agregaron vistas complementarias para Open Dental, Escuela de Influencers y remasterización.
- Los datos procesados están en `src/data/informeFabrica2026.json`.
- El script de extracción de uso puntual está en `scripts/extract_informe_fabrica.py`; no forma parte del runtime.

Archivos principales:

- `src/components/reports/CoberturaFabricaTab.tsx`
- `src/data/informeFabrica2026.json`
- `scripts/extract_informe_fabrica.py`
- `src/pages/Reports.tsx`
- `tsconfig.app.json`

## Capacidad de fábrica

- Se agregó la pestaña **Capacidad** en Reportes.
- La lectura se organiza de micro a macro: persona, proyecto y fábrica.
- Se muestran horas pendientes, capacidad semanal, porcentaje de ocupación y nivel de riesgo.
- Cada proyecto permite abrir un detalle con equipo, carga individual y tareas pendientes.
- Se distingue capacidad de producción directa y capacidad de apoyo por cargo.
- Se listan personas con carga y personas con semana libre.
- Se agregó búsqueda por persona, cargo o proyecto.
- La explicación visual presenta la fórmula con valores reales, un gráfico circular y una escala de riesgo.

Fórmula:

```text
ocupación (%) = horas estimadas pendientes / capacidad semanal × 100
```

Interpretación:

- Menos de 50 %: tiene espacio.
- De 50 % a 89,9 %: cabe en una semana.
- De 90 % a 99,9 %: casi llena.
- Desde 100 %: requiere una semana completa o más.

Consideraciones:

- Se usa todo el backlog sin completar, no solamente lo que vence en la semana actual.
- Una ocupación de 200 % equivale aproximadamente a dos semanas de trabajo acumulado.
- Las tareas sin estimación no incrementan el porcentaje.
- Cuando un perfil no tiene capacidad configurada, estos reportes usan 40,25 horas semanales.
- Los porcentajes entre proyectos no deben sumarse porque una persona puede participar en varios proyectos.

Archivos principales:

- `src/components/reports/CapacidadFabricaTab.tsx`
- `src/hooks/useReports.ts`
- `server/src/controllers/reportsController.ts`
- `server/src/routes/reports.ts`
- `src/pages/Reports.tsx`

## API de reportes

Se agregaron cuatro consultas autenticadas:

- `GET /api/reports/project-utilization`
- `GET /api/reports/project-utilization/:projectId`
- `GET /api/reports/production-capacity`
- `GET /api/reports/people-workload`

Además, se corrigieron conversiones `ROUND(..., 2)` para PostgreSQL en distribución de tiempos, tiempos por fase y detalle de tareas.

## Conexión y autenticación

- El tiempo máximo de conexión inicial a PostgreSQL pasó de 2 a 10 segundos para tolerar bases remotas o arranques lentos.
- Cuando Google OAuth no está configurado, las rutas de acceso redirigen al frontend con `google_error=not_configured`.
- La pantalla de autenticación muestra una explicación y permite continuar con correo y contraseña.

Archivos:

- `server/src/config/database.ts`
- `server/src/routes/auth.ts`
- `src/pages/Auth.tsx`

## Accesibilidad e interfaz

- El diálogo de búsqueda ahora tiene título y descripción para lectores de pantalla.
- El menú lateral móvil incorpora título y descripción accesibles.
- La barra de pestañas de Reportes admite más secciones y salto de línea.
- Se retiró del resumen una alerta duplicada de tareas vencidas.

Archivos:

- `src/components/ui/command.tsx`
- `src/components/ui/sidebar.tsx`
- `src/pages/Reports.tsx`

## Dependencias y compilación

- Los archivos `package-lock.json` reflejan la resolución de dependencias usada por frontend y backend.
- `tsconfig.app.json` habilita `resolveJsonModule` para importar el informe procesado.
- `dist/index.html` contiene el artefacto frontend generado que ya era rastreado por el proyecto.

## Validación realizada

- Compilación del frontend con `npm run build`.
- Verificación de diagnósticos del componente de capacidad sin errores.
- Inicio local del frontend en `http://localhost:8080`.
- Inicio local del backend en `http://localhost:3001`.
- Verificación de `GET /healthz` con respuesta satisfactoria.

## Inventario completo de archivos modificados frente a `main`

- `dist/index.html`
- `package-lock.json`
- `scripts/extract_informe_fabrica.py`
- `server/package-lock.json`
- `server/src/config/database.ts`
- `server/src/controllers/reportsController.ts`
- `server/src/routes/auth.ts`
- `server/src/routes/reports.ts`
- `src/components/reports/CapacidadFabricaTab.tsx`
- `src/components/reports/CoberturaFabricaTab.tsx`
- `src/components/ui/command.tsx`
- `src/components/ui/sidebar.tsx`
- `src/data/informeFabrica2026.json`
- `src/hooks/useReports.ts`
- `src/pages/Auth.tsx`
- `src/pages/Reports.tsx`
- `tsconfig.app.json`

Documentación añadida para la publicación:

- `docs/README.md`
- `docs/CAMBIOS_RAMA_COPIA.md`
- `server/README_REPORTES.md`
- `README.md`

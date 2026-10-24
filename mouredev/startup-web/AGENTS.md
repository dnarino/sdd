# AGENTS.md — startup-web

## Proyecto
Web de la startup: landing page que explica el producto y recoge emails para la lista de espera.
Sitio estático con Astro + Tailwind, un endpoint API para el formulario, Supabase como base de datos y Resend para emails. Desplegado en Vercel.

## Comandos
- Instalar: `npm install`
- Ejecutar: `npm run dev`
- Tests: `npm test` (Vitest) y `npm run test:e2e` (Playwright)
- Lint/formato: `npm run lint` y `npm run format`
- Build: `npm run build`

## Estilo y convenciones
- TypeScript en modo estricto. Node 22 LTS.
- Componentes en PascalCase (`WaitlistForm.astro`), resto de archivos en kebab-case.
- Código, nombres y commits en inglés; textos visibles para el usuario según la spec.
- Commits con Conventional Commits (`feat:`, `fix:`, `docs:`, `test:`…), uno por tarea.

## Reglas
- Lee `docs/constitution.md` y la spec activa (`specs/<NNN-feature>/`) antes de tocar código.
- Sigue el proceso de `docs/sdd-guide.md`: spec → aclarar → plan → tareas → implementar → validar.
- Implementa solo la siguiente tarea sin marcar de `tasks.md`. Nada que no esté en la spec.
- Respeta los **Boundaries** del `plan.md` activo:
  - ✅ Siempre: ejecutar los tests antes de marcar una tarea; claves y secretos solo en `.env`.
  - ⚠️ Preguntar antes: añadir una dependencia; cambiar el modelo de datos; cambiar la spec o el plan.
  - 🚫 Nunca: commitear `.env` o claves; borrar datos; añadir funciones fuera de alcance.

## Al terminar cualquier tarea
- Ejecuta `npm run lint` y `npm test`. Si fallan, la tarea no está terminada.
- Marca la tarea `- [x]` en `tasks.md` y actualiza la tabla de validación si corresponde.
- Resume en 2-3 líneas qué hiciste y cuál es la siguiente tarea.

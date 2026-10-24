# AGENTS.md — Visita con Plan (Shippear #3 · desafío La Virginia)

## Producto
Asistente para el vendedor de La Virginia. En la visita saca 3–6 fotos (góndolas, fachada/instalaciones, precios)
y la IA:
1. Completa sola el relevamiento del comercio (formato, NSE con señales, estado de góndola, cajas, ubicación).
2. Propone el surtido ideal La Virginia (tiene / le falta) y qué ofrecer primero con argumento.
3. Registra precios de productos clave (papel higiénico, aceite, leche, etc.) para comparar entre comercios.
4. Permite repreguntar por chat.
Restricción de negocio: el vendedor tiene ~15 min por cliente. El análisis debe tardar < 1 min y pedir cero tipeo.

## Stack (fijo, no cambiar)
- Next.js App Router + TypeScript + Tailwind + shadcn/ui (generado con v0).
- Supabase (tabla `comercios`, `visitas`, storage `fotos`).
- Vercel AI SDK (`ai` + proveedor) con `generateObject` + zod para salida estructurada.
- Deploy: Vercel, conectado a GitHub. `main` = producción. Cada rama = preview.
- Versiones FIJAS en package.json (nunca "latest").

## Dueños de carpetas (para evitar conflictos)
- v0 (interfaz): `app/page.tsx`, `app/(ui)/**`, `components/**`, estilos.
- Claude (lógica): `app/api/**`, `lib/**`, `supabase/**`, `data/**`, `docs/**`.
- `lib/types.ts` es el contrato compartido: lo define Claude; v0 solo lo importa.

## Contrato
Ver `docs/CONTRATO.md`. Si algo no está en el contrato, no se inventa: se usa el tipo existente o se pregunta.

## Reglas
- Ruta de demo que SIEMPRE debe funcionar: subir fotos → ficha → chat → lista de comercios.
- Claves (API del modelo, Supabase) solo en variables de entorno de Vercel / `.env.local`. Nunca en el código.
- La IA no inventa: si un dato no se ve en las fotos (p. ej. un precio ilegible), devuelve `null` y lo dice.
- Datos semilla precargados para la demo (10–15 comercios) por si falla la red.
- Integración: PR de v0 primero, después PR de Claude. Después de cada merge todos hacen pull.

# Auditoría de seguridad — Quality Gate de Huella Design

**Repositorio público de documentación** para la auditoría del quality gate de seguridad
del proyecto `HuellaDesign/huella-design` (tienda Next.js 16, despliegue en Vercel).

- **Fecha del incidente:** 5 de octubre de 2026
- **Rama del parche:** `fix/security-audit`
- **Pull request:** [#19](https://github.com/HuellaDesign/huella-design/pull/19) (`fix/security-audit` → `main`)
- **Estado:** parche comprometido en la rama, PR abierto sin fusionar, quality gate en rojo pendiente de un cambio de workflow.

---

## 1. Hallazgos del `Security audit` (5 oct 2026)

El job `Quality gate` falló en `main` (commit `58f273b`) porque el paso
`npm audit --audit-level=high` detectó **9 vulnerabilidades**: 8 altas y 1 crítica.

| Severidad | Paquete | Aviso | Descripción |
|---|---|---|---|
| 🔴 Crítica | `next` 16.2.0–16.3.5 | GHSA-vcvr-r3jv-pc5j | Ejecución remota de código (RCE) en Next.js |
| 🟠 Alta | `brace-expansion` 1.1.18 | — | Denegación de servicio (ReDoS) |
| 🟠 Alta | `braces` (anidado 5.0.9) | — | Denegación de servicio vía `brace-expansion` |

Los pasos de tests, lint, typecheck y build de producción no llegaron a ejecutarse
porque la auditoría detuvo el job antes.

## 2. Parche aplicado (rama `fix/security-audit`, commit `d9471c4`)

El commit modifica únicamente `package.json` y `package-lock.json` en la raíz:

| Paquete | Antes | Después |
|---|---|---|
| `next` | 16.3.5 | **16.3.8** ✅ corrige el RCE |
| `eslint-config-next` | 16.3.5 | **16.3.8** (alineado con `next`) |
| `brace-expansion` | 1.1.18 | **1.1.21** ✅ corrige el ReDoS |
| `brace-expansion` anidado | 5.0.9 | **5.0.12** ✅ |

`braces` ya estaba en la versión parcheada 3.0.3: no se tocó (evita un cambio mayor de Tailwind).

## 3. Cambio de workflow pendiente (acción manual requerida)

El gate seguirá en rojo para dependencias de desarrollo sin parche disponible
(7 avisos altos en herramientas de dev sin versión corregida publicada).
Recomendación de un cambio de una línea en `.github/workflows/quality-gate.yml`:
auditar solo dependencias de producción (`npm audit --omit=dev`), de modo que el
gate bloquee por riesgo real en lo que se despliega.

⚠️ Este cambio lo debe hacer el propietario del repositorio manualmente: la
integración de GitHub utilizada no tiene permiso `workflows` (rechazado con HTTP 403).

## 4. Estado actual

- [x] Vulnerabilidad crítica (RCE Next.js) parcheada en la rama
- [x] Vulnerabilidades altas (`brace-expansion`) parcheadas en la rama
- [x] PR #19 creado y abierto
- [ ] PR #19 fusionado a `main`
- [ ] Cambio de workflow aplicado (`.github/workflows/quality-gate.yml`)
- [ ] Quality gate en verde en `main`

---

*Documentación generada el 13 de octubre de 2026. No contiene código fuente ni secretos:
solo el informe de la auditoría y el estado del parche.*

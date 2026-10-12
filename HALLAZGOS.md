# Detalle de vulnerabilidades

## GHSA-vcvr-r3jv-pc5j — Next.js (CRÍTICA)

- **Paquete afectado:** `next` versiones 16.2.0 a 16.3.5
- **Tipo:** Ejecución remota de código (RCE)
- **Vector:** el servidor Next.js en producción
- **Mitigación aplicada:** actualización a `next` 16.3.8 (versión con el parche del aviso)
- **Nota:** `eslint-config-next` se actualizó a 16.3.8 en la misma operación para mantener
  la coherencia de versiones del framework. No hay cambios de código de la aplicación:
  el parche es solo de dependencias.

## brace-expansion — Denegación de servicio (ALTA)

- **Paquete afectado:** `brace-expansion` 1.1.18 (directo) y 5.0.9 (anidado vía `braces`)
- **Tipo:** ReDoS — expresión regular vulnerable que puede colgar el proceso con entradas maliciosas
- **Mitigación aplicada:** `brace-expansion` 1.1.18 → 1.1.21 y anidado 5.0.9 → 5.0.12
- **Nota:** `braces` ya estaba en 3.0.3 (versión parcheada); no se modificó para evitar
  un salto de versión mayor de Tailwind CSS sin necesidad.

## Avisos restantes en dependencias de desarrollo (7, sin parche disponible)

Siete avisos de severidad alta en herramientas de desarrollo sin versión corregida
publicada a fecha de la auditoría. No afectan al código desplegado en producción.
Propuesta: que el workflow audite solo dependencias de producción
(`npm audit --omit=dev --audit-level=high`) para que el gate refleje el riesgo real
del despliegue.

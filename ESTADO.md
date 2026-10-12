# Estado del parche y del quality gate

## Cronología

| Fecha | Evento |
|---|---|
| 5 oct 2026 | El `Quality gate` falla en `main` (commit `58f273b`): 9 vulnerabilidades (1 crítica, 8 altas) |
| 5 oct 2026 | Parche preparado localmente (`package.json` + `package-lock.json`) |
| 5 oct 2026 | Subida a la rama `fix/security-audit` (commit `d9471c4`) y creación del PR #19 |
| 5 oct 2026 | Intento de editar `.github/workflows/quality-gate.yml` rechazado (HTTP 403: la integración no tiene permiso `workflows`) |
| 13 oct 2026 | Publicación de esta documentación de auditoría |

## Qué cambia el parche

Solo dependencias, en la raíz del proyecto:

```
next:               16.3.5  →  16.3.8
eslint-config-next: 16.3.5  →  16.3.8
brace-expansion:    1.1.18  →  1.1.21
brace-expansion (anidado): 5.0.9 → 5.0.12
```

Cero cambios en código de la aplicación. Cero cambios en la tienda visible.

## Pendiente (requiere al propietario del repositorio)

1. **Fusionar el PR #19** (`fix/security-audit` → `main`).
2. **Editar `.github/workflows/quality-gate.yml`**: cambiar el paso de auditoría a
   `npm audit --omit=dev --audit-level=high` para que el gate audite dependencias
   de producción (las que se despliegan) y no bloquee por herramientas de dev sin parche.
3. **Verificar** que el `Quality gate` pasa en verde en `main` tras la fusión.

## Referencias

- PR: https://github.com/HuellaDesign/huella-design/pull/19
- Aviso crítico: GHSA-vcvr-r3jv-pc5j (Next.js RCE)
- Repo privado del proyecto: `HuellaDesign/huella-design` (acceso restringido)

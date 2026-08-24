# Checklist de Release y Despliegue — Python (Docker / PyPI / Cloud)

Lista de verificación obligatoria antes de desplegar aplicaciones de Python en producción o publicar librerías en PyPI.

---

## 📋 1. Checklist Pre-Despliegue (Verificación de Código)

- [ ] **Suite de Pruebas**: Ejecutar `uv run pytest` y confirmar 100% de éxito en tests unitarios e integrados.
- [ ] **Analisis de Estilo**: Ejecutar `uv run ruff check` y confirmar 0 errores de linteo o imports.
- [ ] **Type Checking Estricto**: Ejecutar `uv run mypy src/` y confirmar ausencia de errores de tipado.
- [ ] **Variables de Entorno**: Confirmar que todas las variables requeridas en `config.py` estén documentadas en `.env.example`.

---

## 🔐 2. Seguridad y Gestión de Secretos

- [ ] **Credenciales de API / LLM**: Asegurar que claves de OpenAI, Anthropic o bases de datos **NUNCA** estén escritas en código fuente ni commiteadas en Git.
- [ ] **Escaneo de Dependencias**: Ejecutar auditoría de vulnerabilidades con `uv pip audit` o `safety check`.
- [ ] **Desactivación de Modos Debug**: Confirmar que `DEBUG = False` y la documentación automática `/docs` de FastAPI esté protegida o deshabilitada en entorno de producción público si corresponde.

---

## 📦 3. Compilación de Contenedor Docker

```dockerfile
# Multi-stage Dockerfile optimizado para Python con uv
FROM python:3.12-slim AS builder
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv
WORKDIR /app
COPY pyproject.toml uv.lock ./
RUN uv sync --frozen --no-dev

FROM python:3.12-slim AS runner
WORKDIR /app
COPY --from=builder /app/.venv /app/.venv
COPY src/ /app/src/
ENV PATH="/app/.venv/bin:$PATH"
CMD ["uvicorn", "src.package_name.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

## 🚀 4. Despliegue y Post-Lanzamiento

- [ ] Verificar health check (`GET /healthz`) en el servidor de destino.
- [ ] Monitoreo de logs y métricas de latencia / uso de tokens LLM mediante OpenTelemetry / LangSmith / Sentry.

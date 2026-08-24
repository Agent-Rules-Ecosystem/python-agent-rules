# Estructura Canónica de Proyecto — Python (AI, Data Engineering & Microservicios)

Organización de directorios moderna utilizando la estructura recomendada `src/` layout y gestores de dependencias modernos (`uv` o `poetry`).

---

## 🐍 1. Estructura Estándar de Proyecto Python (src layout)

```text
root/
├── pyproject.toml                # Configuración central (build tool, dependencias, ruff, mypy, pytest)
├── uv.lock (o poetry.lock)       # Lockfile estricto de resolución de dependencias
├── README.md                     # Documentación general del proyecto
├── .env.example                  # Plantilla de variables de entorno (sin credenciales)
├── src/                          # Código fuente empaquetable de Python
│   └── package_name/             # Paquete principal de la aplicación
│       ├── __init__.py
│       ├── main.py               # Punto de entrada de la aplicación (CLI / FastAPI app)
│       ├── config.py             # Gestión de configuración mediante pydantic-settings
│       ├── api/                  # Capa API / Routers (FastAPI endpoints)
│       │   ├── __init__.py
│       │   ├── v1/               # Versionado de APIs (v1, v2)
│       │   │   ├── endpoints/
│       │   │   └── router.py
│       ├── agents/               # Orquestadores de Agentes de IA / Chains (LangChain/LlamaIndex)
│       │   ├── __init__.py
│       │   ├── tools/            # Herramientas expuestas a los LLMs (Tool Calling)
│       │   └── prompts/          # Plantillas de prompts versionadas
│       ├── core/                 # Lógica de dominio pura y casos de uso
│       │   ├── __init__.py
│       │   ├── interfaces/       # Protocolos / Clases abstractas de repositorio
│       │   └── services/         # Servicios de negocio
│       ├── db/                   # Persistencia de datos y conexiones
│       │   ├── __init__.py
│       │   ├── session.py        # Sesión asíncrona de SQLAlchemy / Motor de DB
│       │   └── models/           # Tablas / Entidades ORM (SQLAlchemy v2 / SQLModel)
│       └── schemas/              # Esquemas Pydantic v2 (DTOs de entrada y salida)
│           ├── __init__.py
│           └── user.py
├── tests/                        # Pruebas unitarias e integradas con Pytest
│   ├── __init__.py
│   ├── conftest.py               # Fixtures globales de Pytest (AsyncClient, DB Session)
│   ├── unit/                     # Pruebas unitarias aisladas
│   └── integration/              # Pruebas de API e integración con bases de datos
└── docker/                       # Dockerfiles y scripts de despliegue
    ├── Dockerfile
    └── docker-compose.yml
```

---

## ⚙️ Reglas de Organización para el Agente

1. **`src/` Layout Estricto**: Todo el código de producción DEBE vivir dentro de `src/package_name/`. Evita importar código directamente desde la raíz.
2. **Gestión de Entorno Virtual**: Usar siempre `uv` (`uv venv`, `uv sync`, `uv run`) o `poetry` para aislación de entornos virtuales. NUNCA instalar paquetes globalmente en el sistema.
3. **Pydantic v2 para Configuración**: Usar `pydantic-settings` (`BaseSettings`) para parsear variables de entorno con validación estricta de tipos.

# Guía de Estilo y Buenas Prácticas — Python

Reglas de código limpio, Type Hinting estricto y verificación automatizada mediante **Ruff** y **Mypy** para Python 3.12+.

---

## 🎨 1. Convenciones de Nomenclatura y PEP 8

### Nomenclatura
* **Módulos y Paquetes**: `snake_case` en minúsculas (`user_service.py`, `agent_tools`).
* **Clases y Excepciones**: `PascalCase` (`UserRepository`, `InvalidTokenException`).
* **Funciones, Métodos y Variables**: `snake_case` (`calculate_total()`, `user_id`).
* **Constantes Globales**: `SNAKE_CASE_UPPER` (`MAX_RETRIES = 3`).

### Principios Fundamentales
1. **Type Hints Obligatorios**:
   * Toda función o método DEBE declarar los tipos de entrada y el tipo de retorno (`def fetch_user(user_id: int) -> User | None:`).
   * Utilizar la sintaxis de unión moderna `X | Y` en lugar de `Optional[X]` o `Union[X, Y]`.
2. **Programación Asíncrona (`asyncio`)**:
   * Usar `async def` para endpoints de FastAPI, clientes HTTP (ej. `httpx`) y operaciones de entrada/salida no bloqueantes.
   * Evitar llamadas bloqueantes sincrónicas dentro de funciones asíncronas (usar `asyncio.to_thread` si se invoca código sincrónico pesado).
3. **Pydantic v2**:
   * Usar `BaseModel` de Pydantic v2 para validación de datos. Evitar diccionarios genéricos `dict[str, Any]` como estructuras de datos internas.

---

## 🧪 2. Linters y Formateo Automatizado

### Ruff (Linter & Formatter Ultrarrápido)
En `pyproject.toml`:
```toml
[tool.ruff]
target-version = "py312"
line-length = 88
select = ["E", "F", "I", "B", "UP", "SIM"] # Error, Pyflakes, Isort, Bugbear, PyUpgrade, Simplify

[tool.ruff.format]
quote-style = "double"
```

* Ejecutar formateo y linter: `uv run ruff check --fix` y `uv run ruff format`.

### Mypy (Chequeo Estático de Tipos)
* Configurar `mypy` en modo estricto (`strict = true`) en `pyproject.toml`.
* Ejecutar chequeo: `uv run mypy src/`.

---

## 🛡️ 3. Manejo de Excepciones

* Evitar bloques `except Exception:` genéricos que se tragan errores silenciosamente.
* Definir excepciones personalizadas del proyecto heredando de una excepción base del dominio (`class DomainException(Exception): pass`).
* Documentar las excepciones lanzadas mediante docstrings en formato Google o NumPy.

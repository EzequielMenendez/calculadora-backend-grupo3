# Calculadora — Backend

API REST de una calculadora, desarrollada con FastAPI. Expone las cuatro
operaciones básicas y, cuando hay una base PostgreSQL configurada, conserva un
historial de los cálculos.

## Ejecutar localmente

Requiere Python 3.12 o posterior.

```bash
python -m venv .venv
.venv\\Scripts\\activate
pip install -r requirements.txt -r requirements-dev.txt
uvicorn main:app --reload --port 8000
```

La API queda disponible en `http://127.0.0.1:8000` y su documentación
interactiva en `http://127.0.0.1:8000/docs`.

## Endpoints principales

- `POST /api/calcular`: recibe `a`, `b` y `operacion` (`suma`, `resta`,
  `multiplicacion` o `division`).
- `GET /api/salud`: informa el estado de la API y si el historial está activo.
- `GET /api/historial?limite=10`: devuelve las operaciones guardadas.

## Variables de entorno

| Variable | Uso |
| --- | --- |
| `ORIGENES_PERMITIDOS` | Orígenes del frontend autorizados por CORS, separados por comas. Si no se define, habilita los orígenes locales de desarrollo. |
| `DATABASE_URL` | URL de conexión a PostgreSQL. Es opcional: sin ella la calculadora funciona, pero el historial responde `503`. |

## Tests

Con las dependencias de desarrollo instaladas:

```bash
pytest
```

Los tests usan el cliente en memoria de FastAPI, por lo que no hace falta
levantar el servidor ni disponer de una base de datos local.

## Docker

```bash
docker build -t calculadora-backend .
docker run --rm -p 8000:8000 -e ORIGENES_PERMITIDOS=http://localhost:5500 calculadora-backend
```

Para habilitar el historial, agregá `DATABASE_URL` al contenedor con la URL de
PostgreSQL correspondiente.

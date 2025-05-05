FROM python:3.10

ENV PYTHONUNBUFFERED=1
ENV PYTHONPATH=/app
ENV PORT=8080

WORKDIR /app/

COPY --from=ghcr.io/astral-sh/uv:0.5.11 /uv /uvx /bin/

ENV PATH="/app/.venv/bin:$PATH"

ENV UV_COMPILE_BYTECODE=1
ENV UV_LINK_MODE=copy

COPY . /app/

COPY backend/uv.lock /app/uv.lock
COPY backend/pyproject.toml /app/pyproject.toml
COPY backend/alembic.ini /app/alembic.ini
COPY backend/app /app/app

RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen --no-install-project

RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]

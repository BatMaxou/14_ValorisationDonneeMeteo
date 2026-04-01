FROM dhi.io/python:3.12-dev AS builder

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    UV_NO_DEV=1

COPY --from=ghcr.io/astral-sh/uv:0.10.0 /uv /uvx /bin/

COPY . .
RUN uv sync --locked

FROM dhi.io/python:3.12 AS prod

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PATH="/app/.venv/bin:$PATH"

WORKDIR /app

COPY --from=builder /app /app

USER 1000

EXPOSE 8000

CMD ["gunicorn", "config.wsgi:application", "--bind", "0.0.0.0:8000"]

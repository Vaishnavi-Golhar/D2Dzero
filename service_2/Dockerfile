FROM python:3.11-slim

WORKDIR /app

RUN pip install uv

COPY . .

RUN uv pip install -r <(uv pip compile pyproject.toml)

EXPOSE 8000

CMD ["python", "app.py"]
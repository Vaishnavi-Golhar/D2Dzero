FROM python:3.12-slim

WORKDIR /app

RUN apt update && apt install -y curl

# Install uv
RUN curl -Ls https://astral.sh/uv/install.sh | bash

ENV PATH="/root/.local/bin:$PATH"

COPY . .

# Exclude .venv with .dockerignore before building
RUN uv pip compile pyproject.toml -o requirements.txt
RUN uv pip install --system -r requirements.txt

EXPOSE 8002

CMD ["uv", "run", "app.py"]


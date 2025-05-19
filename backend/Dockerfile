# backend/Dockerfile

# Use official Python base image
FROM python:3.11-slim

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Set work directory
WORKDIR /code

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpq-dev \
    curl \
 && rm -rf /var/lib/apt/lists/*

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | python3 -

ENV PATH="/root/.local/bin:$PATH"

# Copy pyproject.toml and poetry.lock first for caching
COPY pyproject.toml poetry.lock* /code/

# Install Python dependencies inside a virtual environment
RUN poetry config virtualenvs.in-project true && poetry install --no-dev --no-interaction --no-ansi

# Copy backend source code
COPY . /code/

# Expose port 8000 for gunicorn
EXPOSE 8000

# Start gunicorn via poetry virtual environment
CMD ["poetry", "run", "gunicorn", "django_core.core.wsgi:application", "--bind", "0.0.0.0:8000", "--workers", "3"]

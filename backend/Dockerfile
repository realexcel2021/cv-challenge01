# Start with a base image that has Python 3.8
FROM python:3.11-slim

# # Set environment variables to prevent Python from writing .pyc files and enable buffer
# ENV PYTHONDONTWRITEBYTECODE 1
# ENV PYTHONUNBUFFERED 1

# # Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpq-dev \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | python3 -
ENV PATH="/root/.local/bin:$PATH"

# Set the working directory
WORKDIR /backend

# Copy only the pyproject.toml and poetry.lock files to leverage Docker cache
COPY . /backend/

# Install dependencies
RUN poetry install

# Ensure the prestart script is executable
RUN chmod +x ./prestart.sh

# # Run the prestart script
# RUN poetry run bash ./prestart.sh

# Expose the port the app runs on
EXPOSE 8000

# Run the FastAPI application with Uvicorn
CMD ["poetry", "run", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]

# Use a base Python image
FROM python:3.10-slim

# Set working directory
WORKDIR /app

# Install system dependencies required by dlib, CMake, and OpenCV at runtime
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    python3-dev \
    libboost-all-dev \
    libopenblas-dev \
    liblapack-dev \
    libx11-dev \
    libgtk-3-dev \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender1 \
    git \
  && rm -rf /var/lib/apt/lists/*

# Copy & install Python dependencies
COPY requirements.txt .
RUN pip install --upgrade pip \
 && pip install --no-cache-dir -r requirements.txt

# Copy the rest of your code
COPY . .

# Expose the port (Render will set $PORT at runtime)
ENV PORT=8000
EXPOSE 8000

# Use Gunicorn for production; bind to all interfaces on $PORT
CMD ["gunicorn", "app:app", "--bind", "0.0.0.0:$PORT", "--workers", "3"]

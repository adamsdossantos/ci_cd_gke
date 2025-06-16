FROM python:3.9-slim

WORKDIR /app

# Install minimal system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip
RUN pip install --upgrade pip

# Install packages step by step to avoid compilation issues
RUN pip install --only-binary=all numpy==1.24.3
RUN pip install --only-binary=all scipy==1.10.1
RUN pip install --only-binary=all scikit-learn==1.1.3

# Try to install scikit-surprise with pre-built wheel, fallback to source if needed
RUN pip install scikit-surprise==1.1.1 || pip install --no-binary scikit-surprise scikit-surprise==1.1.1

# Copy requirements and install remaining packages
COPY requirements.txt .
RUN pip install --no-cache-dir streamlit==1.28.1 pandas==1.5.3 joblib==1.3.2 pymongo==4.8.0

# Copy the rest of the application
COPY . /app

EXPOSE 8080
ENTRYPOINT ["streamlit", "run", "app.py", "--server.port=8080", "--server.address=0.0.0.0"]
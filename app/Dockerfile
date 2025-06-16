FROM python:3.8-slim

WORKDIR /app

# Install comprehensive build dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    gcc \
    g++ \
    gfortran \
    python3-dev \
    libopenblas-dev \
    liblapack-dev \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

# Install specific pip version known to work
RUN pip install --upgrade pip==23.3.2

# Install build dependencies first
RUN pip install setuptools==68.2.2 wheel==0.41.2 cython==0.29.32

# Install numpy first (required for scikit-surprise compilation)
RUN pip install numpy==1.21.0

# Set environment variables for compilation
ENV CFLAGS="-Wno-error -Wno-misleading-indentation"
ENV CPPFLAGS="-I/usr/local/lib/python3.8/site-packages/numpy/core/include"

# Install scipy
RUN pip install scipy==1.7.0

# Install scikit-surprise with verbose output for debugging
RUN pip install --verbose --no-cache-dir scikit-surprise==1.1.1

# Install remaining packages
RUN pip install streamlit==1.28.1 pandas==1.5.3 scikit-learn==1.1.3 joblib==1.3.2 pymongo==4.8.0

# Copy the rest of the application
COPY . /app

EXPOSE 8080
ENTRYPOINT ["streamlit", "run", "app.py", "--server.port=8080", "--server.address=0.0.0.0"]
# FROM python:3.8-slim
# WORKDIR /app
# RUN apt-get update && \
#     apt-get install -y gcc
# COPY . /app
# RUN ls -al /app 
# RUN pip install -r requirements.txt
# EXPOSE 8080
# ENTRYPOINT ["streamlit", "run", "app.py"]

FROM python:3.8-slim

WORKDIR /app

# Install system dependencies including Python dev headers
RUN apt-get update && apt-get install -y \
    build-essential \
    gcc \
    g++ \
    python3-dev \
    libatlas-base-dev \
    gfortran \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip and install build tools
RUN pip install --upgrade pip setuptools wheel

# Install numpy and cython first (required for scikit-surprise)
RUN pip install --no-cache-dir numpy==1.21.6 cython==0.29.36

# Install scipy separately (often needed for scikit-surprise)
RUN pip install --no-cache-dir scipy==1.7.3

# Use a small script with include_dirs to compile it explicitly
RUN python -c "import numpy; print(numpy.get_include())"  # Confirm headers are there

RUN pip install --no-binary :all: scikit-surprise==1.1.3

# Copy requirements and install remaining packages
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application
COPY . /app

EXPOSE 8080
ENTRYPOINT ["streamlit", "run", "app.py", "--server.port=8080", "--server.address=0.0.0.0"]
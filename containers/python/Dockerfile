# Stage 1: Build Python environment
FROM debian:latest as builder

# Install build dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libffi-dev \
    libssl-dev \
    zlib1g-dev \
    libbz2-dev \
    libreadline-dev \
    libsqlite3-dev \
    wget \
    curl \
    llvm \
    libncurses5-dev \
    xz-utils \
    tk-dev \
    libxml2-dev \
    libxmlsec1-dev \
    libffi-dev \
    liblzma-dev

# Download and build Python
WORKDIR /tmp/python-build
RUN wget https://www.python.org/ftp/python/3.9.6/Python-3.9.6.tar.xz \
    && tar -xf Python-3.9.6.tar.xz \
    && cd Python-3.9.6 \
    && ./configure --enable-optimizations --enable-shared \
    && make -j$(nproc) \
    && make install

# Stage 2: Final image
FROM debian:latest

# Copy the installed Python interpreter from the builder stage
COPY --from=builder /usr/local/bin/python3.9 /usr/local/bin/python3.9
COPY --from=builder /usr/local/lib/python3.9 /usr/local/lib/python3.9

# Set up environment variables
ENV PYTHONPATH=/usr/local/lib/python3.9 \
    PATH=/usr/local/bin:$PATH

# Install additional dependencies if needed
RUN apt-get update && apt-get install -y \
    # Additional packages here, if needed
    && rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /app

# Copy your application code into the container
COPY . .

# Set the entrypoint or default command for your application
CMD [ "python3.9", "your_script.py" ]


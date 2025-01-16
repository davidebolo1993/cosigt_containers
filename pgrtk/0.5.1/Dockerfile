# Build stage - as recommended here: https://github.com/GeneDx/pgr-tk/blob/main/docker/Dockerfile.build_env-22.04
FROM ubuntu:22.04 

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    git \
    ssh \
    curl \
    wget \
    clang-14 \
    cmake \
    libssl-dev \
    unzip \
    libssl3 \
    pkg-config && \
    rm -rf /var/lib/apt/lists/*

# Install Rust and maturin
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"
RUN rustup update
RUN cargo install --locked maturin

# Download and prepare the pgr-tk binary
WORKDIR /opt
RUN mkdir -p pgr-tk && \
    cd pgr-tk && \
    wget https://github.com/GeneDx/pgr-tk/releases/download/v0.5.1/pgr-tk-v0.5.1.zip && \
    unzip pgr-tk-v0.5.1.zip && \
    chmod +x release/*

# Set PATH for binaries
ENV PATH="/opt/pgr-tk/release:$PATH"

# Default command
CMD ["pgr-pbundle-decomp"]


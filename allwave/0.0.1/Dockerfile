# Build stage
FROM ubuntu:latest

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install all necessary build dependencies
# This version resolves the libcurl conflict by pinning the openssl version
RUN apt-get update && \
    apt-get remove -y libcurl4-gnutls-dev || true \
    apt-get install -y --no-install-recommends \
        build-essential \
        cmake \
        autoconf \
        g++ \
        git \
        zlib1g-dev \
        wget \
        make \
        ca-certificates \
        pkg-config \
        libncurses5-dev \
        libncursesw5-dev \
        libbz2-dev \
        libhts-dev \
        libssl-dev \
        liblzma-dev \
        curl \
        # Pin the openssl version to resolve the conflict definitively
        libcurl4-openssl-dev \
    # Clean up apt caches to reduce image size
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install Rust and maturin
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"

RUN rustup update && \
    cargo install --locked maturin

# Set workdir
WORKDIR /opt

# Clone the repository and build the project
RUN git clone https://github.com/pangenome/allwave && \
    cd allwave && \
    cargo build --release

# Add the compiled binary to the PATH
ENV PATH="/opt/allwave/target/release:${PATH}"

# Verify the installation was successful
RUN allwave --help

# Set the default command for the container
CMD ["allwave"]


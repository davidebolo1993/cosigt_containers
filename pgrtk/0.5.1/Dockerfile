# Build stage: Use bullseye-slim as the base image for minimal footprint
FROM debian:bullseye-slim AS build

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
    pkg-config && \
    rm -rf /var/lib/apt/lists/*

# Install Rust and maturin
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y && \
    /root/.cargo/bin/rustup update && \
    /root/.cargo/bin/cargo install --locked maturin

# Download and prepare the pgr-tk binary
WORKDIR /opt
RUN mkdir -p pgr-tk && \
    cd pgr-tk && \
    wget https://github.com/GeneDx/pgr-tk/releases/download/v0.5.1/pgr-tk-v0.5.1.zip && \
    unzip pgr-tk-v0.5.1.zip && \
    chmod +x release/*

# Final stage: Create a minimal image with runtime dependencies
FROM debian:bullseye-slim

# Install runtime dependencies (just the necessary ones)
RUN apt-get update && apt-get install -y --no-install-recommends \
    libstdc++6 \
    libgcc-s1 \
    libc6 \
    ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# Copy the pgr-tk binary and Rust toolchain from the build stage
COPY --from=build /opt/pgr-tk/release /opt/pgr-tk/release
COPY --from=build /root/.cargo/bin /root/.cargo/bin

# Set PATH for binaries
ENV PATH="/opt/pgr-tk/release:/root/.cargo/bin:$PATH"

# Default command
CMD ["pgr-pbundle-decomp"]


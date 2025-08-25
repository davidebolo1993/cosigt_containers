# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && \
    # Explicitly remove the conflicting gnutls package first.
    # The '|| true' ensures the command doesn't fail if the package isn't already present.
    apt-get remove -y libcurl4-gnutls-dev || true && \
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
        libcurl4-openssl-dev && \
    # Clean up apt caches to reduce image size
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install Rust and maturin
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
# Correctly set the PATH for subsequent RUN commands using the recommended syntax
ENV PATH="/root/.cargo/bin:${PATH}"

RUN rustup update && \
    cargo install --locked maturin

# Set workdir
WORKDIR /opt

# Clone the repository and build the project
RUN git clone https://github.com/pangenome/allwave && \
    cd allwave && \
    cargo build --release

# Add the compiled binary to the PATH for the final container image
ENV PATH="/opt/allwave/target/release:${PATH}"

# Verify the installation was successful
RUN allwave --help

# Set the default command for the container
CMD ["allwave"]


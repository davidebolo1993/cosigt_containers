# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    git \
    curl \
    cmake \
    ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# Install Rust and maturin
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"
RUN rustup update
RUN cargo install --locked maturin

#Set workdir
WORKDIR /opt

# Install gafpack
RUN git clone https://github.com/pangenome/gafpack.git \
	&& cd gafpack \
	&& git checkout f08c04b4596bd9c40b0e62641f37c2500b956a1a \
	&& cargo install --force --path .

# Set env path
ENV PATH /opt/gafpack/target/release:$PATH

# Default command
CMD ["gafpack"]

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

# Install gfainject
RUN git clone https://github.com/AndreaGuarracino/gfainject \
	&& cd gfainject \
	&& git checkout 48c98e01b39d0cec71c9130898cbd8fa95bdac80 \
	&& cargo install --force --path .

# Set env path
ENV PATH /opt/gfainject/target/release:$PATH

# Default command
CMD ["gfainject"]

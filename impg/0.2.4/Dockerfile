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
    wget \
    ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# Install Rust and maturin
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"
RUN rustup update
RUN cargo install --locked maturin

#Set workdir
WORKDIR /opt

# Install impg
RUN git clone https://github.com/pangenome/impg \
	&& cd impg \
	&& git checkout 21d70bd7a65651a7be5a87abee66b510d6bd9beb \
	&& cargo install --force --path .

# Set env path
ENV PATH /opt/impg/target/release:$PATH

# Install bedtools
RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.31.0/bedtools.static \
	&& chmod +x bedtools.static \
	&& mv bedtools.static bedtools

#Set path
ENV PATH /opt:$PATH

# Default command
CMD ["impg"]

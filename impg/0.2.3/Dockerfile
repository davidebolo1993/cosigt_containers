# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    git \
    curl

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
	&& git checkout 15248982d5a873b36e5fe4547b265f9f172cfb54 \
	&& cargo install --force --path .

# Set env path
ENV PATH /opt/impg/target/release:$PATH

# Default command
CMD ["impg"]

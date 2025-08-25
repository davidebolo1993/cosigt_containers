# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Advanced dependency installation to bypass stubborn apt conflicts
RUN apt-get update && \
    # First, get a list of all dependency URLs without installing
    apt-get install -y --no-install-recommends --print-uris \
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
        libcurl4-openssl-dev \
    | grep -oP \"'http.*?deb'\" | sed \"s/'//g\" > /tmp/packages.list && \
    # Now, download all packages using wget
    wget --input-file /tmp/packages.list -P /var/cache/apt/archives && \
    # Finally, install the downloaded packages using dpkg
    dpkg -i /var/cache/apt/archives/*.deb && \
    # Clean up
    rm -rf /var/lib/apt/lists/* /tmp/packages.list

# Install Rust and maturin
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"

RUN rustup update && \
    cargo install --locked maturin

# Set workdir
WORKDIR /opt

# Install gfainject
RUN git clone https://github.com/pangenome/allwave && \
    cd allwave && \
    cargo build --release

# Set env path
ENV PATH="/opt/allwave/target/release:${PATH}"

# Example
RUN /opt/allwave/target/release/allwave --help

# Default command
CMD ["allwave"]



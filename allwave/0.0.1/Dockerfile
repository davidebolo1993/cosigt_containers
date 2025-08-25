# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Advanced dependency installation to bypass stubborn apt conflicts
# This version fixes the shell quoting issue from the previous attempt.
RUN apt-get update && \
    # Get a list of all dependency URLs without installing them
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
    | grep -oP "'http.*?\.deb'" | sed "s/'//g" > /tmp/packages.list && \
    # Download all the packages from the list
    wget --input-file /tmp/packages.list -P /var/cache/apt/archives && \
    # Install all the downloaded .deb files directly with dpkg
    dpkg -i /var/cache/apt/archives/*.deb && \
    # Clean up to reduce image size
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/packages.list

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


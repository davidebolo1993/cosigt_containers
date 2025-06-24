# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
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
    liblzma-dev \
    curl \
    libcurl4-openssl-dev \
    && rm -rf /var/lib/apt/lists/*


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
	&& git checkout 889b9f934ef23d3b804a8ff8c085bda2b9224e7c \
	&& cargo install --force --path .

# Set env path
ENV PATH /opt/gfainject/target/release:$PATH

#Install samtools
RUN wget https://github.com/samtools/samtools/releases/download/1.22/samtools-1.22.tar.bz2 \
	&& tar -jxvf samtools-1.22.tar.bz2 \
	&& rm samtools-1.22.tar.bz2 \
	&& cd samtools-1.22 \
	&& ./configure \
	&& make \
	&& make install

# Default command
CMD ["gfainject"]

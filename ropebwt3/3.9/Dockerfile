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
    libopenmpi-dev \
    openmpi-bin \
    libcurl4-openssl-dev \
    && rm -rf /var/lib/apt/lists/*

#Set workdir
WORKDIR /opt

# Install samtools
RUN git clone https://github.com/lh3/ropebwt3 \
	&& cd ropebwt3 \
	&& make

ENV PATH /opt/ropebwt3:$PATH

# Default command
CMD ["ropebwt3"]

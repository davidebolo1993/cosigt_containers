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
    zlib1g-dev \
    wget \
    make \
    ca-certificates \
    pkg-config \
    libncurses5-dev \
    libncursesw5-dev \
    git \
    libbz2-dev \
    liblzma-dev \
    && rm -rf /var/lib/apt/lists/*

#Set workdir
WORKDIR /opt

# Install bwa-mem2
RUN wget https://github.com/bwa-mem2/bwa-mem2/releases/download/v2.2.1/bwa-mem2-2.2.1_x64-linux.tar.bz2 \
	&& tar -jxvf bwa-mem2-2.2.1_x64-linux.tar.bz2 \
	&& rm bwa-mem2-2.2.1_x64-linux.tar.bz2

# Set env path
ENV PATH /opt/bwa-mem2-2.2.1_x64-linux:$PATH

# Install samtools
RUN wget https://github.com/samtools/samtools/releases/download/1.21/samtools-1.21.tar.bz2 \
        && tar -jxvf samtools-1.21.tar.bz2 \
        && rm samtools-1.21.tar.bz2 \
        && cd samtools-1.21 \
        && ./configure \
        && make

# Set env path
ENV PATH /opt/samtools-1.21:$PATH

# Default command
CMD ["bwa-mem2 mem"]

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
    curl \
    libcurl4-openssl-dev \
    && rm -rf /var/lib/apt/lists/*

#Set workdir
WORKDIR /opt

# Install minimap2
RUN wget https://github.com/lh3/minimap2/releases/download/v2.28/minimap2-2.28_x64-linux.tar.bz2 \
	&& tar -jxvf minimap2-2.28_x64-linux.tar.bz2 \
	&& rm minimap2-2.28_x64-linux.tar.bz2

# Set env path
ENV PATH /opt/minimap2-2.28_x64-linux:$PATH

# Install samtools
RUN wget https://github.com/samtools/samtools/releases/download/1.22/samtools-1.22.tar.bz2 \
        && tar -jxvf samtools-1.22.tar.bz2 \
        && rm samtools-1.22.tar.bz2 \
        && cd samtools-1.22 \
        && ./configure \
        && make

# Set env path
ENV PATH /opt/samtools-1.22:$PATH


# Bgzip
RUN wget https://github.com/samtools/htslib/releases/download/1.22/htslib-1.22.tar.bz2 \
	&& tar -jxvf htslib-1.22.tar.bz2 \
	&& rm htslib-1.22.tar.bz2 \
	&& cd htslib-1.22 \
	&& ./configure \
	&& make

ENV PATH /opt/htslib-1.22:$PATH

# Default command
CMD ["minimap2"]

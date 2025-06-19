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
    libcurl-devel \
    && rm -rf /var/lib/apt/lists/*

#Set workdir
WORKDIR /opt

# Install samtools
RUN wget https://github.com/samtools/samtools/releases/download/1.22/samtools-1.22.tar.bz2 \
	&& tar -jxvf samtools-1.22.tar.bz2 \
	&& rm samtools-1.22.tar.bz2 \
	&& cd samtools-1.22 \
	&& ./configure \
	&& make \
	&& make install

# Install htslib
RUN wget https://github.com/samtools/htslib/releases/download/1.22/htslib-1.22.tar.bz2 \
	&& tar -jxvf htslib-1.22.tar.bz2 \
	&& rm htslib-1.22.tar.bz2 \
	&& cd htslib-1.22 \
	&& ./configure \
	&& make \
	&& make install

# Default command
CMD ["samtools"]

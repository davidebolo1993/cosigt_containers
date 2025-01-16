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
    && rm -rf /var/lib/apt/lists/*

#Set workdir
WORKDIR /opt

#install bwa
RUN git clone https://github.com/lh3/bwa.git \
	&& cd bwa \
	&& git checkout 79b230de48c74156f9d3c26795a360fc5a2d5d3b \
	&& make

ENV PATH /opt/bwa:$PATH

# Install also samtools since we use bwa in combination with samtools most of the time
RUN wget https://github.com/samtools/samtools/releases/download/1.21/samtools-1.21.tar.bz2 \
	&& tar -jxvf samtools-1.21.tar.bz2 \
	&& rm samtools-1.21.tar.bz2 \
	&& cd samtools-1.21 \
	&& ./configure \
	&& make

# Set env path
ENV PATH /opt/samtools-1.21:$PATH

# Default command
CMD ["samtools"]

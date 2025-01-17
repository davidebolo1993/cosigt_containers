# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    wget \
    ca-certificates && \
    rm -rf /var/lib/apt/lists/*

#Set workdir
WORKDIR /opt

# Install bwa-mem2
RUN wget https://github.com/bwa-mem2/bwa-mem2/releases/download/v2.2.1/bwa-mem2-2.2.1_x64-linux.tar.bz2 \
	&& tar -jxvf bwa-mem2-2.2.1_x64-linux.tar.bz2 \
	&& rm bwa-mem2-2.2.1_x64-linux.tar.bz2

ENV PATH /opt/bwa-mem2-2.2.1_x64-linux:$PATH

# Default command
CMD ["bwa-mem2 mem"]

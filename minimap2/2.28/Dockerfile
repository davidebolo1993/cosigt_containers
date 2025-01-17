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

# Install minimap2
RUN wget https://github.com/lh3/minimap2/releases/download/v2.28/minimap2-2.28_x64-linux.tar.bz2 \
	&& tar -jxvf minimap2-2.28_x64-linux.tar.bz2 \
	&& rm minimap2-2.28_x64-linux.tar.bz2

ENV PATH /opt/minimap2-2.28_x64-linux:$PATH

# Default command
CMD ["minimap2"]

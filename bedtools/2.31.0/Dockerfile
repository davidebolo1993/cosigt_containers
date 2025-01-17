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

# Install bedtools
RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.31.0/bedtools.static \
	&& chmod +x bedtools.static \
	&& mv bedtools.static bedtools

#Set path
ENV PATH /opt:$PATH

# Default command
CMD ["bedtools"]

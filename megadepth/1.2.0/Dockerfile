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

# Install megadepth
## install megadepth
RUN wget https://github.com/ChristopherWilks/megadepth/releases/download/1.2.0/megadepth \
	&& chmod +x megadepth

# Default command
CMD ["megadepth"]

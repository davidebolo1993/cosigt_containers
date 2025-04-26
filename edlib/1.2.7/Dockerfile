# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    cmake \
    make \
    git \
    g++ \
    wget \
    ca-certificates \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

#Set workdir
WORKDIR /opt

#Install edlib
RUN git clone https://github.com/Martinsos/edlib.git \
	&& cd edlib/build \
	&& cmake -D CMAKE_BUILD_TYPE=Release .. \
	&& make \
	&& make install

#compile the script
RUN wget https://raw.githubusercontent.com/davidebolo1993/cosigt/refs/heads/master/cosigt_smk/workflow/scripts/compute_qv.cpp \
	&& g++ compute_qv.cpp -ledlib -o compute_qv

# Set env path
ENV PATH="/opt:${PATH}"

# Default command
ENTRYPOINT ["compute_qv"]


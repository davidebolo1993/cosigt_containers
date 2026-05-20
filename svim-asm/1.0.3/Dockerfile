# Build stage
FROM debian:bullseye-slim AS binary

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    curl \
    ca-certificates && \
    rm -rf /var/lib/apt/lists/*

#Build python conda env
RUN curl -LO https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
RUN bash Miniconda3-latest-Linux-x86_64.sh -p /miniconda -b && \
    rm Miniconda3-latest-Linux-x86_64.sh
ENV PATH=/miniconda/bin:${PATH}
RUN conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main \
 && conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r
RUN conda update -y conda && \
        conda create -y -n svimasmenv -c conda-forge -c bioconda \
	svim-asm=1.0.3
RUN echo "source activate svimasmenv" > ~/.bashrc
ENV PATH /miniconda/envs/svimasmenv/bin:$PATH


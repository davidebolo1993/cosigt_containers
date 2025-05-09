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

#Build r conda env
RUN curl -LO https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
RUN bash Miniconda3-latest-Linux-x86_64.sh -p /miniconda -b && \
    rm Miniconda3-latest-Linux-x86_64.sh
ENV PATH=/miniconda/bin:${PATH}
RUN conda update -y conda && \
        conda create -y -n renv -c conda-forge -c bioconda -c r \
	r-base=4.3.3 \
	r-data.table=1.16.4 \
	r-rjson=0.2.23 \
	r-reshape2=1.4.4 \
	r-nbclust=3.0.1 \
	r-ggplot2=3.5.1 \
	r-dendextend=1.18.1 \
	r-gggenes=0.5.1 \
	r-devtools=2.4.5 \
	bioconductor-rtracklayer=1.62.0 \
	r-randomcolor=1.1.0.1 \
        r-pafr=0.0.2 \
        r-tidyverse=2.0.0 \
        r-r.utils=2.12.3 \
	r-ggnewscale \
	r-ggforce \
	bioconductor-s4vectors \
	bioconductor-bsgenome \
	bioconductor-genomicranges \
	bioconductor-biostrings \
	bioconductor-iranges \
	bioconductor-genomeinfodb \
	bioconductor-rsamtools \
	r-wesanderson \
	r-abind=1.4_5 \
	r-xml \
	r-v8 \
	r-dbscan=1.2.2 \
	r-curl \
	r-ggrepel \
	r-cowplot
RUN echo "source activate renv" > ~/.bashrc
ENV PATH /miniconda/envs/renv/bin:$PATH
RUN echo "devtools::install_github('daewoooo/SVbyEye', branch='master', upgrade='never')" > install.r && \
	Rscript install.r

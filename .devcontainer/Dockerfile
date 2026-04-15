# Version arguments
# ARG variables only persist during build time
ARG FASTQC_VERSION="v0.11.9_cv8"


FROM mambaorg/micromamba:2.5.0-ubuntu22.04 AS app

ARG FASTQC_VERSION

# Example: setting an environment variable or running a command with it
RUN echo "Successfully pulled FastQC version: ${FASTQC_VERSION}"

# build and run as root users since micromamba image has 'mambauser' set as the $USER
USER root
# set workdir to default for building; set to /data at the end
WORKDIR /

LABEL base.image="biocontainers/fastqc:v0.11.9_cv8"
LABEL dockerfile.version="1"
LABEL software="FastQC"
LABEL software.version="${FASTQC_VERSION}"
LABEL description="FastQC is a quality control tool for high throughput sequence data."
LABEL website="https://www.bioinformatics.babraham.ac.uk/projects/fastqc/"  
LABEL license="https://www.bioinformatics.babraham.ac.uk/projects/fastqc/license.html"
LABEL maintainer="Your Name"
LABEL maintainer.email=""

# Set the environment path
ENV PATH='/opt/conda/envs/fastqc/bin:${PATH}'\
    LC_ALL=C.UTF-8

# Install FastQC using conda
RUN mamba create -n fastqc -y -c bioconda -c conda-forge fastqc=${FASTQC_VERSION} && \
    mamba clean -a -y

# Set the working directory to /data
WORKDIR /data

# make directory editable
RUN chmod -R a+rwX /opt/conda/envs/fastqc

# default command to run when container starts
CMD ["fastqc", "--help"]

# new base for testing
FROM app AS test

RUN fastqc --help && \
    fastqc --version

# install dependencies; cleanup apt garbage
RUN apt-get update && apt-get install -y --no-install-recommends \
    procps \
    && rm -rf /var/lib/apt/lists/*

# Grab test data from FastQC's GitHub repository
RUN apt-get update && apt-get install -y --no-install-recommends && \
    wget -O complex.fastq https://github.com/s-andrews/FastQC/blob/master/test/data/complex.fastq && \
    wget -O minimal.fastq https://github.com/s-andrews/FastQC/blob/master/test/data/minimal.fastq

# Run FastQC on the test data
RUN fastqc complex.fastq minimal.fastq -o /data/test_output && \
    ls -l /data/test_output

# check that the expected output files are present
RUN head -n 1 /data/test_output/complex_fastqc.zip && \
    head -n 1 /data/test_output/minimal_fastqc.zip

# list installed packages and versions for testing purposes
RUN conda list -n fastqc

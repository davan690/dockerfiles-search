FROM ubuntu:16.04

# apt dependency
RUN apt-get update && \
    apt-get install -y cmake build-essential gcc g++ git wget && \

# python-package
    # miniconda
    wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
    /bin/bash Miniconda3-latest-Linux-x86_64.sh -f -b -p /opt/conda && \
    export PATH="/opt/conda/bin:$PATH" && \
    # rgf_python
    conda install -y numpy scipy scikit-learn pandas && \
    git clone --recursive https://github.com/fukatani/rgf_python.git && \
    cd rgf_python && python setup.py install && \

# clean
    apt-get autoremove -y && apt-get clean && \
    conda clean -i -l -t -y && \
    rm -rf /usr/local/src/*

ENV PATH /opt/conda/bin:$PATH


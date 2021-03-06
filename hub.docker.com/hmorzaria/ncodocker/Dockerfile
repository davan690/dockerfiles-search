FROM rocker/tidyverse
MAINTAINER hmorzaria@hotmail.com
# Install minimum requirements
RUN apt-get update -qq && apt-get -y --no-install-recommends install \
    libhdf5-dev \
    libnetcdf-dev \
    libgdal-dev \
    libudunits2-dev \
    nco \
    cdo
    
#Install AzCopy, ver. 7.2 includes .NET Core dependencies; they do not need to install them a pre-requisite
#https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-linux
RUN \
    apt-get update && apt-get -y upgrade &&\
    apt-get -y install rsync wget &&\
    wget -O azcopy.tar.gz https://aka.ms/downloadazcopylinuxrhel6 &&\
    tar -xzf azcopy.tar.gz &&\
    ./install.sh &&\
    rm -Rf azcopy* install.sh
    
    #setup R configs
RUN echo "r <- getOption('repos'); r['CRAN'] <- 'http://cran.us.r-project.org'; options(repos = r);" > ~/.Rprofile
RUN Rscript -e "install.packages('digest')"
RUN Rscript -e "install.packages('tidyverse')"
RUN Rscript -e "install.packages('data.table')"
RUN Rscript -e "install.packages('rgdal')"
RUN Rscript -e "install.packages('magrittr')"
RUN Rscript -e "install.packages('raster')"
RUN Rscript -e "install.packages('sp')"
RUN Rscript -e "install.packages('fields')"
RUN Rscript -e "install.packages('ncdf4')"
RUN Rscript -e "install.packages('RNetCDF')"

ENV TZ America/Los_Angeles
RUN ln -snf /usr/share/timezone/$TZ /etc/localtime && echo $TZ > /etc/timezone
CMD ["R"]

FROM centos:centos7


# Update System Image and install EPEL
RUN \
  yum update -y && \
  yum upgrade -y && \
  yum install -y epel-release

# Install OpenJDK7 and R
RUN \
  yum install -y java-1.7.0-openjdk-headless R
  
  #install additional tools and library prerequisites
RUN \
  yum install -y unzip netcdf-devel libxml2-devel ImageMagick graphviz cairo-devel libXt-devel 
  
# Setup default cran repo
RUN echo "r <- getOption('repos'); r['CRAN'] <- 'http://cran.us.r-project.org'; options(repos = r);" > ~/.Rprofile

CMD "/bin/bash"
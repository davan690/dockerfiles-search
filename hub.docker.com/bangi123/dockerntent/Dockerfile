FROM ubuntu:18.04
MAINTAINER BananaBb

# Install common tools
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update  \ 
 && apt-get install -y \ 
 	dialog \ 
	sudo \ 
	zip \ 
	unzip \ 
	bzip2 \ 
	wget \ 
	curl \ 
	git \ 
	nano \ 
	vim \ 
	htop

CMD ["/bin/bash"]
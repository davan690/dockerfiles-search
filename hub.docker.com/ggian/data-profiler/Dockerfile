FROM debian

MAINTAINER Giannis Giannakopoulos

ENV GOPATH="/opt" GIMME_GO_VERSION="1.7" R_LIBS="/opt/rlibs"

RUN apt-get update && \
	apt-get install -y \
		curl \ 
		gcc \
		git \
		r-base \
		r-cran-igraph \
		sqlite3 && \
	echo "install.packages('GenSA', repos='http://cran.us.r-project.org')" | R --vanilla && \
	echo "install.packages('e1071', repos='http://cran.us.r-project.org')" | R --vanilla && \
	echo "install.packages('neuralnet', repos='http://cran.us.r-project.org')" | R --vanilla && \
	curl -sL -o gimme https://raw.githubusercontent.com/travis-ci/gimme/master/gimme && \
	chmod +x ./gimme

RUN eval $(./gimme) && \
	mkdir -p $GOPATH/src/github.com/giagiannis && \
	cd $GOPATH/src/github.com/giagiannis && \
	git clone -b devel https://github.com/giagiannis/data-profiler.git && \
	cd data-profiler && \
	go get -v ./... && \
	go install -v ./...

ADD entrypoint.sh /
ADD data-profiler /etc/data-profiler

EXPOSE 8080

ENTRYPOINT "/entrypoint.sh"

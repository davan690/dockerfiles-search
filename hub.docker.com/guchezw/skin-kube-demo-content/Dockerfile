FROM openjdk:8-jre-alpine


# wget -c -t 0 https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v5.6.10/elasticsearch-analysis-ik-5.6.10.zip

# wget -c -t 0 https://github.com/medcl/elasticsearch-analysis-pinyin/releases/download/v5.6.10/elasticsearch-analysis-pinyin-5.6.10.zip

# ensure elasticsearch user exists
RUN addgroup -S elasticsearch && adduser -S -G elasticsearch elasticsearch

# grab su-exec for easy step-down from root
# and bash for "bin/elasticsearch" among others
RUN apk add --no-cache 'su-exec>=0.2' bash

# https://artifacts.elastic.co/GPG-KEY-elasticsearch
ENV GPG_KEY 46095ACC8548582C1A2699A9D27D666CD88E42B4

WORKDIR /usr/share/elasticsearch
ENV PATH /usr/share/elasticsearch/bin:$PATH

ENV ELASTICSEARCH_VERSION 5.6.10
ENV ELASTICSEARCH_TARBALL="https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.10.tar.gz" \
	ELASTICSEARCH_TARBALL_ASC="https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.10.tar.gz.asc" \
	ELASTICSEARCH_TARBALL_SHA1="c4df8b240635484f09487a66707a9192bf97d3f9"

RUN set -ex; \
	\
	apk add --no-cache --virtual .fetch-deps \
		ca-certificates \
		gnupg \
		openssl \
		tar \
		unzip \
	; \
	\
	wget -O elasticsearch.tar.gz "$ELASTICSEARCH_TARBALL"; \
	\
	if [ "$ELASTICSEARCH_TARBALL_SHA1" ]; then \
		echo "$ELASTICSEARCH_TARBALL_SHA1 *elasticsearch.tar.gz" | sha1sum -c -; \
	fi; \
	\
	if [ "$ELASTICSEARCH_TARBALL_ASC" ]; then \
		wget -O elasticsearch.tar.gz.asc "$ELASTICSEARCH_TARBALL_ASC"; \
		export GNUPGHOME="$(mktemp -d)"; \
		gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$GPG_KEY"; \
		gpg --batch --verify elasticsearch.tar.gz.asc elasticsearch.tar.gz; \
		rm -rf "$GNUPGHOME" elasticsearch.tar.gz.asc; \
	fi; \
	\
	tar -xf elasticsearch.tar.gz --strip-components=1; \
	rm elasticsearch.tar.gz; \
	\
	wget -c -t 0 -O elasticsearch-analysis-ik-${ELASTICSEARCH_VERSION}.zip https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v${ELASTICSEARCH_VERSION}/elasticsearch-analysis-ik-${ELASTICSEARCH_VERSION}.zip; \
	unzip elasticsearch-analysis-ik-${ELASTICSEARCH_VERSION}.zip -d ./plugins; \
	mv ./plugins/elasticsearch ./plugins/elasticsearch-analysis-ik-${ELASTICSEARCH_VERSION}; \
    wget -c -t 0 -O elasticsearch-analysis-pinyin-${ELASTICSEARCH_VERSION}.zip https://github.com/medcl/elasticsearch-analysis-pinyin/releases/download/v${ELASTICSEARCH_VERSION}/elasticsearch-analysis-pinyin-${ELASTICSEARCH_VERSION}.zip; \
    unzip elasticsearch-analysis-pinyin-${ELASTICSEARCH_VERSION}.zip -d ./plugins; \
    mv ./plugins/elasticsearch ./plugins/elasticsearch-analysis-pinyin-${ELASTICSEARCH_VERSION}; \
	apk del .fetch-deps; \
	\
	# mkdir -p ./plugins; \
	for path in \
		./data \
		./logs \
		./config \
		./config/scripts \
	; do \
		mkdir -p "$path"; \
		chown -R elasticsearch:elasticsearch "$path"; \
	done; \
	\
# we shouldn't need much RAM to test --version (default is 2gb, which gets Jenkins in trouble sometimes)
	export ES_JAVA_OPTS='-Xms32m -Xmx32m'; \
	if [ "${ELASTICSEARCH_VERSION%%.*}" -gt 1 ]; then \
		elasticsearch --version; \
	else \
# elasticsearch 1.x doesn't support --version
# but in 5.x, "-v" is verbose (and "-V" is --version)
		elasticsearch -v; \
	fi

COPY config ./config

# VOLUME /usr/share/elasticsearch/data

COPY docker-entrypoint.sh /

RUN echo 'http.cors.enabled: true' >> /usr/share/elasticsearch/config/elasticsearch.yml 
RUN echo 'http.cors.allow-origin: "*" ' >> /usr/share/elasticsearch/config/elasticsearch.yml 

EXPOSE 9200 9300
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["elasticsearch"]

# docker build -t elasticsearch-alpine-ik-pinyin-5.6.10 -f Dockerfile .

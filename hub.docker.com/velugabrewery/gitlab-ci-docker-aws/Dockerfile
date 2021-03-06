ARG NODEJS_VERSION
FROM mhart/alpine-node:${NODEJS_VERSION} as node

FROM library/docker:stable
ARG CONTAINER_ARCHITECTURE=linux-amd64
ARG AWS_CLI_VERSION
ARG ECS_CLI_VERSION
ARG S3_CMD_VERSION
ARG DOCKER_COMPOSE_VERSION
ARG CHAMBER_VERSION

# aws-cli uses 'less -R'. However less with R option is not available in alpine linux
ENV PAGER=more

# groff is required by aws-cli
RUN apk add --no-cache -v --virtual .build-deps \
    py-pip \
    && apk add -v \
        bash \
        curl \
        git \
        groff \
        jq \
        make \
        mysql-client \
        python \
        python-dev \
        python3 \
        python3-dev \
        py-setuptools \
        musl-dev \
        zip \
        gettext \
        gcc \
    && pip3 install --upgrade \
        awscli==${AWS_CLI_VERSION} \
        s3cmd==${S3_CMD_VERSION} \
        docker-compose==${DOCKER_COMPOSE_VERSION} \
        python-magic \
        pipenv \
        pre-commit \
    && curl -o /usr/local/bin/ecs-cli https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-${CONTAINER_ARCHITECTURE}-v${ECS_CLI_VERSION} \
    && chmod +x /usr/local/bin/ecs-cli \
    && curl -Lo /usr/local/bin/chamber https://github.com/segmentio/chamber/releases/download/v${CHAMBER_VERSION}/chamber-v${CHAMBER_VERSION}-linux-amd64 \
    && chmod +x /usr/local/bin/chamber \
&& rm -r /root/.cache \
&& apk del -v .build-deps \
&& rm /var/cache/apk/*

# link python3 as default
RUN if [ ! -e /usr/bin/pip ]; then ln -s pip3 /usr/bin/pip ; fi && \
    if [[ ! -e /usr/bin/python ]]; then ln -sf /usr/bin/python3 /usr/bin/python; fi

# Install Node.js
# https://github.com/mhart/alpine-node#example-dockerfile-for-your-own-nodejs-project
COPY --from=node /usr/lib/libgcc* /usr/lib/libstdc* /usr/lib/
COPY --from=node /usr/lib/node_modules /usr/lib/node_modules

COPY --from=node /usr/bin/node /usr/bin/
COPY --from=node /usr/local/share/yarn /usr/local/share/yarn

RUN ln -s /usr/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm \
&& ln -s /usr/local/share/yarn/bin/yarn /usr/local/bin/ \
&& ln -s /usr/local/share/yarn/bin/yarnpkg /usr/local/bin/

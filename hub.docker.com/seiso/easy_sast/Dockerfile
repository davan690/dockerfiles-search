## Global
ARG ARG_FROM_IMAGE=python
ARG ARG_FROM_IMAGE_TAG=3.8-alpine

## Builder stage
# https://hub.docker.com/_/python?tab=tags
FROM python:3.8 AS builder
ARG ARG_VENDOR=veracode
ENV ENV_VENDOR=${ARG_VENDOR}
WORKDIR /usr/src/app/
# requirements.txt is separated to improve caching
COPY "./${ENV_VENDOR}/requirements.txt" "/usr/src/app/${ENV_VENDOR}/requirements.txt"
ENV PATH=/root/.local/bin:$PATH
RUN pip3 install --user -r "${ENV_VENDOR}/requirements.txt"

## CI stage
FROM builder AS ci
# requirements.txt is separated to improve caching
COPY ./requirements.txt /usr/src/app/requirements.txt
RUN pip3 install --user -r requirements.txt

## Lint Docker
# https://hub.docker.com/r/hadolint/hadolint/tags
FROM hadolint/hadolint:v1 AS lint_docker
WORKDIR /usr/src/app/
ENTRYPOINT ["hadolint"]
CMD ["Dockerfile"]

## Lint git
FROM ci AS lint_git
ENTRYPOINT ["gitlint"]
CMD ["--commits", "HEAD"]

## Lint Makefile
# https://hub.docker.com/r/cytopia/checkmake/tags
FROM cytopia/checkmake:0.1.0 AS lint_make
WORKDIR /usr/src/app/
ENTRYPOINT ["checkmake"]
CMD ["Makefile"]

## Lint Python
FROM ci AS lint_python
ENTRYPOINT find . -type f -name '*.py' -exec pylint -j 0 {} +

## Lint yaml
FROM ci AS lint_yaml
ENTRYPOINT find . -type f \( -name '*.yml' -o -name '*.yaml' \) -exec yamllint {} +

## Type Annotations Linter
#FROM ci AS lint_types
#ENTRYPOINT find "${ENV_VENDOR}" -type f -name '*.py' -exec mypy {} +

## Complexity Linter
#FROM ci AS lint_complexity
#ENTRYPOINT find "${ENV_VENDOR}" -type f -name '*.py' -exec xenon --max-absolute B {} +

## Unit Tests
FROM ci AS test_unit
ENTRYPOINT ["coverage"]
CMD ["run", "-m", "unittest", "discover", "-s", "tests", "-p", "test_*.py"]

## Security Tests
FROM ci AS test_security
ENTRYPOINT find . -type f -name '*.py' -exec bandit {} + && \
  trufflehog --regex --entropy=False file:///usr/src/app/ --exclude_paths .truffleHog-exclude.txt

## easy_sast
FROM "${ARG_FROM_IMAGE}":"${ARG_FROM_IMAGE_TAG}" AS Final

ARG ARG_VERSION
ARG ARG_VENDOR

LABEL MAINTAINER="Seiso"
LABEL AUTHOR="Jon Zeolla"
LABEL COPYRIGHT="(c) 2020 Seiso, LLC"
LABEL LICENSE="BSD-3-Clause"
LABEL VERSION="${ARG_VERSION}"

WORKDIR /usr/src/app/

COPY "./${ARG_VENDOR}" "${ARG_VENDOR}"
COPY ./main.py main.py
COPY --from=builder /root/.local /root/.local

ENV PATH="/root/.local/bin:${PATH}"

# Assumes that the compiled files/debug symbols are in a folder which is volume
# mapped to /build
ENTRYPOINT ["/usr/src/app/main.py"]

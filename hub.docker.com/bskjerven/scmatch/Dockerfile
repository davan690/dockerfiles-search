FROM python:2.7

# Build arguments
# Build date and git commit are dynamically generated by a build hook
ARG BUILD_DATE
ARG VCS_REF
ARG SCMATCH_VER=1.0

# OCI Annotations (https://github.com/opencontainers/image-spec/blob/master/annotations.md)
LABEL org.opencontainers.image.created = "${BUILD_DATE}"
LABEL org.opencontainers.image.authors = "Brian Skjerven <brian.skjerven@pawsey.org.au"
LABEL org.opencontainers.image.revision = "${VCS_REF}"
LABEL org.opencontainers.image.version = "${SCMATCH_VER}"
LABEL org.opencontainers.image.licenses = "MIT"
LABEL org.opencontainers.image.title = "scMatch"
LABEL org.opencontainers.image.description = "This Docker image provides a Python 2 environment and \
  the single cell annotation tool scMatch"
#LABEL org.opencontainers.image.url
#LABEL org.opencontainers.image.documentation
#LABEL org.opencontainers.image.source
#LABEL org.opencontainers.image.ref.name

# Install required packages and clean up
RUN apt-get update -qq && apt-get -y --no-install-recommends install \
      unzip \
      && apt-get clean all \
      && rm -rf /var/lib/apt/lists/*

WORKDIR /scmatch-build
RUN git clone https://github.com/asrhou/scMatch /scmatch-build \
      && pip install -r requirements.txt \
      && pip install \
              openpyxl \
              plotly \
              scikit-learn \
              seaborn \
              umap-learn \
      && cd refDB/FANTOM5 \
      && unzip '*.zip' \
      && rm *.zip \
      && cd /scmatch-build \
      && cp -r refDB / \
      && chmod +x scMatch.py && cp scMatch.py /usr/local/bin \
      && chmod +x toTerms.py && cp toTerms.py /usr/local/bin \
      && chmod +x visAnnos.py && cp visAnnos.py /usr/local/bin

WORKDIR /
RUN rm -rf /scmatch-build

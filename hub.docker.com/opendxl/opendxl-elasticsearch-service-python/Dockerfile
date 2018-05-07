# Base image from Python 2.7 (slim)
FROM python:2.7-slim

VOLUME ["/opt/dxlelasticsearchservice-config"]

# Copy application files
COPY . /tmp/build
WORKDIR /tmp/build

# Clean application
RUN python ./clean.py

# Install application package and its dependencies
RUN pip install .

# Cleanup build
RUN rm -rf /tmp/build

################### INSTALLATION END #######################
#
# Run the application.
#
# NOTE: The configuration files for the application must be
#       mapped to the path: /opt/dxlelasticsearchservice-config
#
# For example, specify a "-v" argument to the run command
# to mount a directory on the host as a data volume:
#
#   -v /host/dir/to/config:/opt/dxlelasticsearchservice-config
#
CMD ["python", "-m", "dxlelasticsearchservice", "/opt/dxlelasticsearchservice-config"]

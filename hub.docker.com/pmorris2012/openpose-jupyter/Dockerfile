FROM cwaffles/openpose-python
MAINTAINER Paul Morris (pmorris2012@fau.edu)

RUN apt-get update

#commands to configure openpose

#move the pyopenpose python library into the python3 package folder so it can be imported.
RUN cp /openpose/build/python/openpose/pyopenpose.cpython-36m-x86_64-linux-gnu.so /usr/local/lib/python3.6/dist-packages/

#download default openpose model weights
RUN chmod +x /openpose/models/getModels.sh
RUN /openpose/models/getModels.sh

#make the directory where external files will be mounted
WORKDIR /external

# Add Tini. Tini operates as a process subreaper for jupyter. This prevents
# kernel crashes.
ENV TINI_VERSION v0.6.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini
ENTRYPOINT ["/usr/bin/tini", "--"]

EXPOSE 8888
CMD ["jupyter", "notebook", "--port=8888", "--no-browser", "--ip=0.0.0.0", "--allow-root", "--NotebookApp.token=''", "--NotebookApp.password=''"]

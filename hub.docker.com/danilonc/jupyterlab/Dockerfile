FROM nvidia/opencl

# Install OS packages
RUN DEBIAN_FRONTEND=non-interactive apt-get update -y \
    && DEBIAN_FRONTEND=non-interactive apt-get dist-upgrade -y \
    && DEBIAN_FRONTEND=non-interactive apt-get install -y python3-pip build-essential cmake git python3-ipython \
    ipython3 libopenmpi-dev python3-dev zlib1g-dev sudo npm nodejs wget\
    && DEBIAN_FRONTEND=non-interactive apt-get autoremove -y

# Install PIP packages
RUN pip3 install jupyterlab stable-baselines tensorflow-gpu gmaps
RUN jupyter lab build && jupyter labextension install @jupyter-widgets/jupyterlab-manager

# Create user, add it to sudo group
RUN useradd -m user && echo user:user | chpasswd && usermod -aG sudo user
RUN echo "user ALL=(ALL:ALL) NOPASSWD:ALL" >> /etc/sudoers

# Default User / Workdir
USER user
WORKDIR /home/user
RUN mkdir /home/user/work

# docker run --rm -it -v "$PWD:/home/user/work" --gpus=all -p 8888:8888 danilonc/jupyterlab
ENTRYPOINT [ "/usr/local/bin/jupyter-lab", "--ip","0.0.0.0"]

FROM archlinux
LABEL authors="Cédric Farinazzo <cedrc.farinazzo@gmail.com>"

#Base installation
RUN pacman --noconfirm -Syyu

RUN pacman --noconfirm -S wget curl sudo fakeroot file patch
RUN pacman --noconfirm -S git make gcc clang cmake
RUN pacman --noconfirm -S valgrind
RUN pacman --noconfirm -S python-pip

# Add user, group sudo
RUN /usr/sbin/groupadd --system sudo && \
    /usr/sbin/useradd -m --groups sudo user && \
    /usr/sbin/sed -i -e "s/Defaults    requiretty.*/ #Defaults    requiretty/g" /etc/sudoers && \
    /usr/sbin/echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers


# Install yay - https://github.com/Jguer/yay
ENV yay_version=9.4.2
ENV yay_folder=yay_${yay_version}_x86_64
RUN cd /tmp && \
    curl -L https://github.com/Jguer/yay/releases/download/v${yay_version}/${yay_folder}.tar.gz | tar zx && \
    install -Dm755 ${yay_folder}/yay /usr/bin/yay && \
    install -Dm644 ${yay_folder}/yay.8 /usr/share/man/man8/yay.8

# Install criterion
RUN sudo -u user yay -S --mflags --nocheck --noconfirm criterion

# Clean cache
RUN pacman --noconfirm -Sc

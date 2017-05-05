FROM ubuntu

RUN apt-get update &&\
    apt-get install -y sudo git-core subversion build-essential gcc-multilib \
                       libncurses5-dev zlib1g-dev gawk flex gettext wget unzip python
RUN apt-get clean
RUN useradd -m openwrt
RUN mkdir -p /etc/sudoers.d
RUN echo 'openwrt ALL=NOPASSWD: ALL' > /etc/sudoers.d/openwrt 
RUN sudo -iu openwrt git clone git://git.openwrt.org/14.07/openwrt.git 

WORKDIR /home/openwrt/openwrt
RUN ./scripts/feeds update -a
RUN ./scripts/feeds install -a

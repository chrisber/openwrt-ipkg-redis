############################################################
# Dockerfile for builtroot Development environment
# Based on Ubuntu Image
############################################################

FROM ubuntu:14.04
# Set the file maintainer (your name - the file's author)
MAINTAINER Christian Karl Bernasko


RUN apt-get update &&\
    apt-get install -y libssl-dev git xsltproc genext2fs subversion build-essential gcc-multilib \
                       libncurses5-dev zlib1g-dev gawk flex gettext wget unzip python sudo &&\
    apt-get clean
RUN useradd -m openwrt
RUN mkdir -p /etc/sudoers.d
RUN echo 'openwrt ALL=NOPASSWD: ALL' > /etc/sudoers.d/openwrt
RUN sudo -iu openwrt git clone git://git.openwrt.org/15.05/openwrt.git
# RUN sudo -iu openwrt openwrt/scripts/feeds update
# RUN sudo -iu openwrt openwrt/scripts/feeds update
ADD ./.config .config
RUN chown openwrt:openwrt .config
RUN ls -la .config

USER openwrt
    WORKDIR /home/openwrt/openwrt/
    RUN ./scripts/feeds update -a
    RUN ./scripts/feeds install -a
    RUN make oldconfig
    RUN make defconfig
    RUN make oldconfig
    RUN make -j 1


USER root
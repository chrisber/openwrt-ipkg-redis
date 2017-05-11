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


USER openwrt
    WORKDIR /home/openwrt/
        RUN git clone http://git.openwrt.org/openwrt.git
    WORKDIR /home/openwrt/openwrt
        # RUN git reset --hard 51e07d09ae86a202b398d5bbb0ec4df9bb18fd4d
USER root
    WORKDIR /home/openwrt/openwrt/
        ADD ./.config .config
         RUN chown openwrt:openwrt .config
USER openwrt
    WORKDIR /home/openwrt/openwrt/
        RUN ./scripts/feeds update -a
        RUN ./scripts/feeds install -a
        RUN make oldconfig
        RUN make -j 4

USER root

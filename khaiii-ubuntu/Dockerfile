FROM ubuntu:16.04
MAINTAINER won

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update -y

# UTF-8 Setting
RUN apt-get install -y locales
RUN locale-gen 		en_US.UTF-8
ENV LANG					en_US.UTF-8
ENV LC_ALL 				en_US.UTF-8

# UTC -> KST
RUN ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime

RUN apt-get install -y software-properties-common
RUN apt-get update -y

RUN apt-get install -y vim bash-completion unzip curl less sudo git net-tools

## adduser
RUN adduser --disabled-password --gecos "" w
RUN usermod -a -G www-data w
RUN usermod -a -G sudo w
RUN echo w ALL=NOPASSWD: ALL >> /etc/sudoers

USER w

RUN sudo git clone https://github.com/kakao/khaiii.git
RUN sudo apt-get install python3-pip -y
RUN pip3 install cmake
RUN sudo mkdir build
WORKDIR /khaiii/build
RUN pwd
RUN sudo ~/.local/lib/python3.5/site-packages/cmake/data/bin/cmake -DCMAKE_INSTALL_PREFIX=/path/to/install ..
RUN sudo make all
RUN sudo make resource
RUN sudo make install
# RUN sudo mkae package_python
# WORKDIR /khaii/build/package_python


EXPOSE 80 443

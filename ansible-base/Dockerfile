FROM centos:8

USER root

# Enable systemd (see: https://hub.docker.com/_/centos/)
ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

# Use base URLs
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

# Install basics
RUN echo "root:root" | chpasswd && \
    yum -y update && \
    yum -y install epel-release && \
    yum -y install sudo openssh-clients openssh-server && \
    yum -y clean all

# Install tooling
RUN yum -y install sudo which nano curl tree && \
    yum -y clean all

# Create ansible user
RUN useradd -ms /bin/bash ansible && \
    echo "ansible:ansible" | chpasswd && \
    echo "ansible ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/ansible

# Install python
RUN yum -y install python3 python3-pip
RUN python3 -m pip install --upgrade pip

# Install ansible
USER ansible

RUN python3 -m pip install --user ansible
RUN python3 -m pip install --upgrade --user ansible

USER root

# Install AEM pre-requisites
RUN yum -y install java-11-openjdk httpd

# VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]
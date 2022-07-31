FROM centos:7

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

CMD ["/usr/sbin/init"]
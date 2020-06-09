ARG VERSION=20.04
FROM qnimbus/ubuntu:$VERSION
MAINTAINER B. van Wetten <bas@vanwetten.com>

ARG VERSION
RUN echo $VERSION > image_version

ENV pip_packages "ansible"

##
## CONFIGURE
##

RUN apt-get-min update

# Install ssh server and client
RUN apt-get-install-min openssh-server openssh-client

# Install Ansible dependencies
RUN apt-get-install-min \
    python3-setuptools  \
    python3-pip         \
    rsyslog             \
    sshpass             \
    sudo                \
    systemd-cron        \
    iproute2

# Install Ansible via Pip.
RUN pip3 install $pip_packages

# Install Ansible inventory file.
RUN mkdir -p /etc/ansible
RUN echo "[local]\nlocalhost ansible_connection=local" > /etc/ansible/hosts

# Create the PrivSep empty dir if necessary
RUN mkdir -p /var/run/sshd
RUN chmod 0755 /var/run/sshd

# Set root password.
RUN echo 'root:docker.io' | chpasswd

# Allow ssh root login.
RUN sed -i 's/#PermitRootLogin.*$/PermitRootLogin yes/' /etc/ssh/sshd_config

##
## CLEANUP
##

# Do not run udev
RUN true \
    && systemctl mask systemd-udev-trigger.service \
    && systemctl mask systemd-udevd.service \
    && systemctl mask systemd-udevd.socket \
    && systemctl mask systemd-udevd-kernel.socket \
    && true

# Mask services that are not required and/or might interfere.
RUN true \
    && systemctl mask getty.target \
    && systemctl mask dbus.service \
    && systemctl mask dbus.socket \
    && systemctl mask rsyslog.service \
    && systemctl mask systemd-initctl.socket \
    && systemctl mask systemd-logind.service \
    && systemctl mask systemd-resolved.service \
    && systemctl mask systemd-timesyncd.service \
    && systemctl mask systemd-journald.service \
    && systemctl mask ssystemd-journald.socket \
    && systemctl mask ssystemd-journald-audit.socket \
    && true

RUN apt-cleanup

##
## INIT
##

EXPOSE 22
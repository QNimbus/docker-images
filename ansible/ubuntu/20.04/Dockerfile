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

# Remove unnecessary getty and udev targets that result in high CPU usage when using
# multiple containers with Molecule (https://github.com/ansible/molecule/issues/1104)
RUN rm -f /lib/systemd/system/systemd*udev* && \
    rm -f /lib/systemd/system/getty.target

RUN apt-cleanup

##
## INIT
##

EXPOSE 22

VOLUME ["/sys/fs/cgroup", "/tmp", "/run"]

CMD ["/usr/sbin/sshd", "-D"]
ARG VERSION=20.04
FROM qnimbus/ubuntu:$VERSION
MAINTAINER B. van Wetten <bas@vanwetten.com>

ARG VERSION
RUN echo $VERSION > image_version

##
## CONFIGURE
##

RUN apt-get-min update

# Install ssh server and client
RUN apt-get-install-min openssh-server openssh-client

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

RUN apt-cleanup

##
## INIT
##

CMD ["/usr/sbin/sshd", "-D"]
FROM centos:centos7

RUN yum -y update && yum clean all

RUN yum -y install openssh-server

RUN ssh-keygen -f /etc/ssh/ssh_host_rsa_key -N '' -t ed25519
RUN ssh-keygen -f /etc/ssh/ssh_host_ecdsa_key -N '' -t ecdsa

RUN sed -ri 's/^#PermitEmptyPasswords no/PermitEmptyPasswords yes/' /etc/ssh/sshd_config

# set empty root user password
RUN echo "root:" | chpasswd

EXPOSE 22

ENTRYPOINT ["/usr/sbin/sshd", "-D"]
FROM anshuljaiswal/ubuntu:14.04-1.0

ARG   RootUser=root
ARG   RootPassword=root

ENV   RootUser=${RootUser}
ENV   RootPassword=${RootPassword}

# Installing Required Packages
RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y \
       openssh-server

RUN mkdir /var/run/sshd \
    && echo ${RootUser}:${RootPassword} | chpasswd \
    && sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
# SSH login fix. Otherwise user is kicked off after login
    && sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd \
    && apt-get autoremove \
    && apt-get clean  \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/apt/archives

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]

# This Image is created for ease of use for developers, This image can be used for earlier stage of production
#
FROM ubuntu:16.04

LABEL MAINTAINER=anshul.a.jaiswal@gmail.com

ARG   RootUser=root 
ARG   RootPassword=root
ARG   CustomUser=docker 
ARG   CutomUserPassword=docker 
ARG   CustomGroup=docker 
ARG   CustomUid=1000 
ARG   CustomGid=1000
ARG   CustomUserHome=/home/${CustomUser}
ARG   OptVolume=/opt

ENV     RootUser=${RootUser} \
        RootPassword=${RootPassword} \
        CustomUser=${CustomUser} \
        CutomUserPassword=${CutomUserPassword} \
        CustomGroup=${CustomGroup} \
        CustomUserHome=${CustomUserHome} \
        OptVolume=${OptVolume}

# Installing Required Packages

RUN apt-get update \
    && apt-get install -y  --no-install-recommends \
      #curl \
      git \
      openssh-server \
      subversion \
      sudo 
      #vim \
      #wget 

# Creating Custom User
# Same thing can be obtained by following 3 approaches

## Approach 1 (Sudo user)

#RUN mkdir -p ${CustomUserHome} \
#   && chown ${CustomUid}:${CustomGid} ${CustomUserHome} \
#   && groupadd -g ${CustomGid} ${CustomGroup} \
#   && useradd -d ${CustomUserHome} -u ${CustomUid} -g ${CustomGid} -m -s /bin/bash ${CustomUser} \
#   && echo ${CustomUser}:${CutomUserPassword} | chpasswd \
#   && adduser ${CustomUser} sudo

## Approach 2 (Sudo User)

#RUN useradd -m ${CustomUser} --uid=${CustomUid} \
#   && echo "${CustomUser}:${CutomUserPassword}" | chpasswd \
#   && adduser ${CustomUser} sudo   

## Approach 3 (Sudo User)
## comment following 3 lines and uncomment next 2 lines if you don't need sudo user
RUN useradd -m ${CustomUser} -s /bin/bash \
    && echo ${CustomUser}:${CutomUserPassword} | chpasswd \
    && adduser ${CustomUser} sudo 

#RUN useradd -m ${CustomUser} \
#    && echo ${CustomUser}:${CutomUserPassword} | chpasswd \

###Mounting Volumes
VOLUME ${CustomUserHome}
VOLUME ${OptVolume}

# Starting SSH Service

RUN mkdir /var/run/sshd \
    && echo ${RootUser}:${RootPassword} | chpasswd \
    && sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
### SSH login fix. Otherwise user is kicked off after login
    && sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd \
    && apt-get autoremove \
    && apt-get clean  \
    && chown -R ${CustomUser}:${CustomGroup} ${OptVolume} \
    && chown -R ${CustomUser}:${CustomGroup} ${CustomUserHome} \    
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/apt/archives 
    
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
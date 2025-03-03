#FROM ubuntu:16.04
#ENV MYSQL_DATABASE DOCKERDB
#

FROM ubuntu:22.04

ARG USERNAME=developer
ARG USER_GROUP_NAME=developer
ARG USER_UID=1000
ARG USER_GID=1000

ARG PKG="git vim curl unzip zip sudo build-essential libz-dev zlib1g-dev ssh"

SHELL ["/bin/bash", "-c"]


RUN apt-get update \
    && apt-get install -y ${PKG} \
    && groupadd --gid ${USER_GID} ${USER_GROUP_NAME} \
    && useradd --uid ${USER_UID} --shell /bin/bash --gid ${USER_GID} -m ${USERNAME} \
    && echo %${USER_GROUP_NAME} ALL=\(ALL\) NOPASSWD:ALL > /etc/sudoers.d/${USER_GROUP_NAME} \
    && chmod 0440 /etc/sudoers.d/${USER_GROUP_NAME}


USER ${USERNAME}
RUN mkdir -p /home/${USERNAME}/.ssh && \
    mkdir -p /home/${USERNAME}/.m2 && \
    mkdir -p /home/${USERNAME}/.aws && \
    mkdir -p /home/${USERNAME}/.nvm

#COPY id_ed* /home/${USERNAME}/.ssh/
#COPY settings.xml /home/${USERNAME}/.m2/
#COPY config /home/${USERNAME}/.aws/
#COPY credentials /home/${USERNAME}/.aws/

RUN curl -s "https://get.sdkman.io" | bash \
    && echo sdkman_curl_connect_timeout=500 >> ~/.sdkman/etc/config \
    && echo sdkman_curl_max_time=500 >> ~/.sdkman/etc/config

RUN source "$HOME/.sdkman/bin/sdkman-init.sh" \
    && sdk install java 17.0.10-amzn \
    && sdk install quarkus 3.6.6 \
    && sdk install maven 3.9.6

RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash \
    && source "$HOME/.nvm/nvm.sh" \
    && nvm install v18.19.1

RUN echo 'shopt -s histappend' >> /home/${USERNAME}/.bashrc && \
    echo 'PROMPT_COMMAND=history -a;$PROMPT_COMMAND' >> /home/${USERNAME}/.bashrc && \
    sed -i 's/OSH_THEME="font"/OSH_THEME="powerline-plain"/g' /home/${USERNAME}/.bashrc && \
    echo 'HISTFILE=/home/developer/.bash_history' >> /home/${USERNAME}/.bashrc && \
    echo alias ll=\'ls -alF\' >> /home/${USERNAME}/.bashrc
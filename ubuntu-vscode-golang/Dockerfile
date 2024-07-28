##
## DEVELOPMENT
##

FROM ubuntu:22.04 AS development
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
    && apt -y upgrade \
    && apt -y install \
              locales \
              rsync \
              openssh-server \
              procps \
              wget \
              unzip \
              mc \
              ca-certificates \
              curl \
              software-properties-common \
              bash-completion \
              shellcheck \
              whois \
              make \
    && add-apt-repository ppa:longsleep/golang-backports \
    && add-apt-repository ppa:git-core/ppa \
    && echo 'deb [trusted=yes] https://repo.goreleaser.com/apt/ /' | tee /etc/apt/sources.list.d/goreleaser.list \
    && apt update \
    && apt -y install \
              git \
              openssl \
              golang-go \
              goreleaser \
              nfpm \
              protobuf-compiler \
              tzdata \
              sudo \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8 \
    && chsh -s /bin/bash

SHELL [ "/bin/bash", "-c" ]

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=${USER_UID}
ARG WORKDIR=/home/vscode/apphome 
# Create the user
RUN echo "source /usr/share/bash-completion/completions/git" >> ~/.bashrc \
    && source ~/.bashrc \
    && groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
# ********************************************************
# * Anything else you want to do like clean up goes here *
# ********************************************************
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# [Optional] Set the default user. Omit if you want to keep the default as root.
USER ${USERNAME}

ENV LC_ALL=en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US.UTF-8
ENV SHELL=/bin/bash
#GO ENV
ENV BASE=/usr/local
ENV GO11MODULE=on
ENV GOPATH=/home/${USERNAME}/go
ENV PATH=$PATH:$GOPATH/bin

# ADD https://github.com/golang/go/raw/master/lib/time/zoneinfo.zip /zoneinfo.zip
# ENV ZONEINFO /zoneinfo.zip

RUN curl -sSL https://rover.apollo.dev/nix/latest | sh \
  && git config --add --global url."git@github.com:".insteadOf https://github.com \
  && go install github.com/uudashr/gopkgs/v2/cmd/gopkgs@latest \
  && go install github.com/ramya-rao-a/go-outline@latest \
  && go install github.com/cweill/gotests/gotests@latest \
  && go install github.com/fatih/gomodifytags@latest \
  && go install github.com/josharian/impl@latest \
  && go install github.com/haya14busa/goplay/cmd/goplay@latest \
  && go install github.com/go-delve/delve/cmd/dlv@latest \
  && go install honnef.co/go/tools/cmd/staticcheck@latest \
  && go install golang.org/x/tools/gopls@latest \
  && go install golang.org/x/tools/cmd/goimports@latest \
  && go install github.com/rogpeppe/godef@latest \
  && go install google.golang.org/protobuf/cmd/protoc-gen-go@latest \
  && go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest \
  && go clean -cache -modcache

WORKDIR "${WORKDIR}"
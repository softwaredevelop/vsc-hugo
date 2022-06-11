ARG VARIANT=latest
FROM docker.io/library/alpine:$VARIANT

RUN \
    apk update; \
    apk add --no-cache sudo; \
    rm -rf /var/cache/apk/*

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN \
    addgroup --gid $USER_GID $USERNAME; \
    adduser -G $USERNAME \
    -h /home/$USERNAME \
    -s /bin/ash \
    -u $USER_UID \
    -D \
    $USERNAME; \
    echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME; \
    chmod 0440 /etc/sudoers.d/$USERNAME

ARG ASSET=hugo
ARG VERSION=latest

SHELL ["/bin/ash", "-eo", "pipefail", "-c"]
RUN \
    apk update; \
    apk add --no-cache \
    curl; \
    rm -rf /var/cache/apk/*; \
    case "${VERSION}" in \
    latest) \
    VERSION=$(curl --silent https://api.github.com/repos/gohugoio/hugo/releases/latest | grep "tag_name" | awk '{print substr($2, 3, length($2)-4)}') \
    export VERSION ;;\
    esac; \
    case $(uname -m) in \
    aarch32) \
    export ARCH=ARM ;;\
    aarch64) \
    export ARCH=ARM64 ;;\
    i686) \
    export ARCH=32bit ;;\
    x86_64) \
    export ARCH=64bit ;;\
    esac; \
    curl -O -L -C - https://github.com/gohugoio/hugo/releases/download/v${VERSION}/${ASSET}_${VERSION}_Linux-${ARCH}.tar.gz; \
    tar -xf ${ASSET}_${VERSION}_Linux-${ARCH}.tar.gz; \
    mv hugo /usr/bin/hugo; \
    rm ${ASSET}_${VERSION}_Linux-${ARCH}.tar.gz; \
    rm -rf /var/cache/apk/*

EXPOSE 1313

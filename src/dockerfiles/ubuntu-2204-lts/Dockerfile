ARG PLATFORM="linux/arm64"

## {{{
ARG GAP_USER_USERNAME="gap"
ARG GAP_USER_GROUPNAME="gap"
ARG GAP_USER_UID=1000
ARG GAP_USER_GID=1000
ARG GAP_USER_SHELL="/bin/bash"
ARG GAP_USER_HOMEDIR="/opt/gap"
## }}}

## {{{
ARG GAP_VERSION="4.13.1"
ARG PACKAGE_MANAGER_VERSION="1.3.2"
## }}}

## {{{
ARG GAP_TARBALL_NAME="gap-${GAP_VERSION}-core.tar.gz"
ARG GAP_DOWNLOAD_URL="https://github.com/gap-system/gap/releases/download/v${GAP_VERSION}/${GAP_TARBALL_NAME}"
## }}}

## {{{
ARG PACKAGE_MANAGER_TARBALL_NAME="PackageManager-${PACKAGE_MANAGER_VERSION}.tar.gz"
ARG PACKAGE_MANAGER_DOWNLOAD_URL="https://github.com/gap-packages/PackageManager/releases/download/v${PACKAGE_MANAGER_VERSION}/${PACKAGE_MANAGER_TARBALL_NAME}"
## }}}




FROM --platform=${PLATFORM} ubuntu:22.04 AS base
ARG GAP_USER_GID
ARG GAP_USER_GROUPNAME
ARG GAP_USER_UID
ARG GAP_USER_USERNAME
ARG GAP_USER_HOMEDIR

RUN groupadd --system --gid ${GAP_USER_GID} ${GAP_USER_GROUPNAME} && \
     useradd --system --gid ${GAP_USER_GID} --home-dir ${GAP_USER_HOMEDIR} --create-home ${GAP_USER_USERNAME}




FROM base AS build
ARG GAP_VERSION
ARG GAP_DOWNLOAD_URL
ARG GAP_TARBALL_NAME
ARG GAP_USER_HOMEDIR
ARG GAP_USER_USERNAME
ARG GAP_USER_GROUPNAME

RUN apt-get update --yes &&                               \
    apt-get install --no-install-recommends --quiet --yes \
    autoconf                                              \
    build-essential                                       \
    cmake                                                 \
    g++                                                   \
    gcc                                                   \
    libgmp3-dev                                           \
    libncurses5-dev curl                                  \
    libreadline6-dev                                      \
    libtool                                               \
    m4                                                    \
    unzip 

USER ${GAP_USER_USERNAME}
WORKDIR ${GAP_USER_HOMEDIR}
ADD --chown=${GAP_USER_USERNAME}:${GAP_USER_GROUPNAME} ${GAP_DOWNLOAD_URL} ./
RUN tar -xzf ${GAP_TARBALL_NAME}

WORKDIR "./gap-${GAP_VERSION}"
RUN ./configure && MAKEFLAGS=-j8 make




FROM base AS result

ARG GAP_VERSION
ARG GAP_USER_HOMEDIR
ARG GAP_USER_USERNAME
ARG GAP_USER_GROUPNAME
ARG PACKAGE_MANAGER_DOWNLOAD_URL

ENV PATH="${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}/:${PATH}"
ENV LD_LIBRARY_PATH="${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}/lib"

RUN apt-get update --yes &&                               \
    apt-get install --no-install-recommends --quiet --yes \
        ca-certificates                                   \
        curl                                              \
        4ti2                                              \
        libcdd0d                                          \
        libcurl4                                          \
        libgmp10                                          \
        libmpc3                                           \
        libmpfi0                                          \
        libmpfr6                                          \
        libncurses5                                       \
        libreadline8                                      \
        libzmq5                                           \
        pari-gp                                           \
        singular                                          \
        zlib1g

USER ${GAP_USER_USERNAME}

RUN mkdir -pv "$HOME/.gap/pkg"
COPY --chown=${GAP_USER_USERNAME}:${GAP_USER_GROUPNAME} --from=build "${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}/" "${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}//"
ADD --chown=${GAP_USER_USERNAME}:${GAP_USER_GROUPNAME} ${GAP_DOWNLOAD_URL} ./

CMD [ "bash" ]

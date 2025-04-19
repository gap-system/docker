## {{{
ARG GAP_VERSION="4.13.1"
ARG PACKAGE_MANAGER_VERSION="1.6.2"
## }}}

## {{{
ARG GAP_USER_USERNAME="gap"
ARG GAP_USER_GROUPNAME="gap"
ARG GAP_USER_UID=1000
ARG GAP_USER_GID=1000
ARG GAP_USER_SHELL="/bin/bash"
ARG GAP_USER_HOMEDIR="/opt/gap"
## }}}

## {{{
ARG GAP_CORE_TARBALL_NAME="gap-${GAP_VERSION}-core.tar.gz"
ARG GAP_CORE_DOWNLOAD_URL="https://github.com/gap-system/gap/releases/download/v${GAP_VERSION}/${GAP_CORE_TARBALL_NAME}"

ARG GAP_TARBALL_NAME="gap-${GAP_VERSION}.tar.gz"
ARG GAP_DOWNLOAD_URL="https://github.com/gap-system/gap/releases/download/v${GAP_VERSION}/${GAP_TARBALL_NAME}"
## }}}

## {{{
ARG PACKAGE_MANAGER_TARBALL_NAME="PackageManager-${PACKAGE_MANAGER_VERSION}.tar.gz"
ARG PACKAGE_MANAGER_DOWNLOAD_URL="https://github.com/gap-packages/PackageManager/releases/download/v${PACKAGE_MANAGER_VERSION}/${PACKAGE_MANAGER_TARBALL_NAME}"
## }}}

FROM ubuntu:22.04 AS base
ARG GAP_USER_GID
ARG GAP_USER_GROUPNAME
ARG GAP_USER_UID
ARG GAP_USER_USERNAME
ARG GAP_USER_HOMEDIR

RUN groupadd --system --gid ${GAP_USER_GID} ${GAP_USER_GROUPNAME} && \
     useradd --system --gid ${GAP_USER_GID} --home-dir ${GAP_USER_HOMEDIR} --create-home ${GAP_USER_USERNAME}

RUN apt-get update --yes && \
    apt-get install --no-install-recommends --quiet --yes \
        ca-certificates                                   \
        curl                                              \
        git                                               \
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
        zlib1g                                            \
        unzip

FROM base AS buildbare
ARG GAP_VERSION
ARG GAP_CORE_DOWNLOAD_URL
ARG GAP_CORE_TARBALL_NAME
ARG GAP_USER_HOMEDIR
ARG GAP_USER_USERNAME
ARG GAP_USER_GROUPNAME

RUN apt-get update --yes &&                               \
    apt-get install --no-install-recommends --quiet --yes \
    autoconf                                              \
    build-essential                                       \
    cmake                                                 \
    zlib1g-dev                                            \
    libgmp-dev                                            \
    libncurses-dev                                        \
    libreadline-dev                                       \
    libtool                                               \
    m4

USER ${GAP_USER_USERNAME}
WORKDIR ${GAP_USER_HOMEDIR}
ADD --chown=${GAP_USER_USERNAME}:${GAP_USER_GROUPNAME} ${GAP_CORE_DOWNLOAD_URL} ./
RUN tar -xzf ${GAP_CORE_TARBALL_NAME}

WORKDIR "./gap-${GAP_VERSION}"
RUN ./configure && MAKEFLAGS=-j8 make

FROM base AS bare
ARG GAP_VERSION
ARG GAP_USER_HOMEDIR
ARG GAP_USER_USERNAME
ARG GAP_USER_GROUPNAME
ARG PACKAGE_MANAGER_TARBALL_NAME
ARG PACKAGE_MANAGER_DOWNLOAD_URL

ENV PATH="${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}/:${PATH}"
ENV LD_LIBRARY_PATH="${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}/lib"

RUN apt-get update --yes &&                               \
    apt-get install --no-install-recommends --quiet --yes \
        ca-certificates                                   \
        curl                                              \
        gpg                                               \
        jq                                                \
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

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN mkdir -pv "$HOME/.gap/pkg"
COPY --chown=${GAP_USER_USERNAME}:${GAP_USER_GROUPNAME} --from=buildbare "${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}/" "${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}//"
ADD --chown=${GAP_USER_USERNAME}:${GAP_USER_GROUPNAME} ${PACKAGE_MANAGER_DOWNLOAD_URL} ./
RUN tar -xvf ${PACKAGE_MANAGER_TARBALL_NAME} -C "$HOME/.gap/pkg"
RUN echo 'LoadPackage("PackageManager"); UpdatePackage("PackageManager", false); QuitGap(InstallRequiredPackages());' | gap --bare
CMD [ "bash" ]

FROM base AS buildslim
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
    libreadline-dev                                       \
    libcdd-dev                                            \
    libcurl4-gnutls-dev                                   \
    libfplll-dev                                          \
    libgmp-dev                                            \
    libmpc-dev                                            \
    libmpfi-dev                                           \
    libmpfr-dev                                           \
    libncurses-dev                                        \
    libtool                                               \
    libzmq3-dev                                           \
    netbase                                               \
    pari-gp                                               \
    perl                                                  \
    polymake                                              \
    singular                                              \
    zlib1g-dev                                            \
    libboost-dev

USER ${GAP_USER_USERNAME}
WORKDIR ${GAP_USER_HOMEDIR}
ADD --chown=${GAP_USER_USERNAME}:${GAP_USER_GROUPNAME} ${GAP_DOWNLOAD_URL} ./
RUN tar -xzf ${GAP_TARBALL_NAME}

WORKDIR "./gap-${GAP_VERSION}"
RUN ./configure && MAKEFLAGS=-j8 make

FROM buildslim AS buildfull
ARG GAP_VERSION
ARG GAP_DOWNLOAD_URL
ARG GAP_TARBALL_NAME
ARG GAP_USER_HOMEDIR
ARG GAP_USER_USERNAME
ARG GAP_USER_GROUPNAME

WORKDIR "${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}/pkg/"
RUN ../bin/BuildPackages.sh --parallel

FROM base AS slim

ARG GAP_VERSION
ARG GAP_USER_HOMEDIR
ARG GAP_USER_USERNAME
ARG GAP_USER_GROUPNAME
ARG PACKAGE_MANAGER_TARBALL_NAME
ARG PACKAGE_MANAGER_DOWNLOAD_URL

ENV PATH="${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}/:${PATH}"
ENV LD_LIBRARY_PATH="${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}/lib"

USER ${GAP_USER_USERNAME}

COPY --chown=${GAP_USER_USERNAME}:${GAP_USER_GROUPNAME} --from=buildslim "${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}/" "${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}//"
CMD [ "bash" ]

FROM base AS full

ARG GAP_VERSION
ARG GAP_USER_HOMEDIR
ARG GAP_USER_USERNAME
ARG GAP_USER_GROUPNAME
ARG PACKAGE_MANAGER_TARBALL_NAME
ARG PACKAGE_MANAGER_DOWNLOAD_URL

ENV PATH="${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}/:${PATH}"
ENV LD_LIBRARY_PATH="${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}/lib"

USER ${GAP_USER_USERNAME}

COPY --chown=${GAP_USER_USERNAME}:${GAP_USER_GROUPNAME} --from=buildfull "${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}/" "${GAP_USER_HOMEDIR}/gap-${GAP_VERSION}//"
CMD [ "bash" ]

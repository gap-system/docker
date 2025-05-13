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

FROM ghcr.io/limakzi/gap-docker:4.12.1-full AS base
EXPOSE 8888

ARG GAP_USER_GID
ARG GAP_USER_GROUPNAME
ARG GAP_USER_UID
ARG GAP_USER_USERNAME
ARG GAP_USER_HOMEDIR

USER 'root'

RUN apt-get -y clean && \
    apt-get -y update && \
    apt-get -y install python3-pip

USER ${GAP_USER_USERNAME}
ENV PATH="/opt/gap/.local/bin/:/opt/gap/gap-4.12.1/pkg/jupyterkernel/bin/:${PATH}"
ENV JUPYTER_GAP_EXECUTABLE "gap"

COPY ./requirements.txt ./
RUN pip3 install -r requirements.txt
RUN pip3 install --no-dependencies /opt/gap/gap-4.12.1/pkg/jupyterkernel/

WORKDIR "/tmp"

CMD ["jupyter-lab", "--ip=0.0.0.0"]

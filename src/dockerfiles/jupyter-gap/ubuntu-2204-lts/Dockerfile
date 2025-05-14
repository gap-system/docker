## {{{
ARG GAP_VERSION="4.12.1"
ARG DOCKER_BASE_IMAGE_URL="ghcr.io/gap-system/gap:${GAP_VERSION}-full"
## }}}

## {{{
ARG JUPYTER_KERNEL_PATH="/opt/gap/gap-${GAP_VERSION}/pkg/jupyterkernel/"
## }}}

## {{{
ARG GAP_USER_USERNAME="gap"
ARG GAP_USER_GROUPNAME="gap"
ARG GAP_USER_UID=1000
ARG GAP_USER_GID=1000
ARG GAP_USER_SHELL="/bin/bash"
ARG GAP_USER_HOMEDIR="/opt/gap"
## }}}

FROM ${DOCKER_BASE_IMAGE_URL} AS full

EXPOSE 8888

ARG GAP_USER_GID
ARG GAP_USER_GROUPNAME
ARG GAP_USER_UID
ARG GAP_USER_USERNAME
ARG GAP_USER_HOMEDIR
ARG JUPYTER_KERNEL_PATH

USER 'root'

RUN apt-get -y clean && \
    apt-get -y update && \
    apt-get -y install python3-pip

USER ${GAP_USER_USERNAME}
ENV PATH="/opt/gap/.local/bin/:${JUPYTER_KERNEL_PATH}/bin/:${PATH}"
ENV JUPYTER_GAP_EXECUTABLE "gap"

COPY ./requirements.txt ./
RUN pip3 install -r requirements.txt
RUN pip3 install --no-dependencies ${JUPYTER_KERNEL_PATH}

WORKDIR "/tmp"

CMD ["jupyter-lab", "--ip=0.0.0.0"]

FROM node:lts-buster

ENV NPM_CACHE "/app/.npm-cache"

RUN set -ex \
    ## update packages and install deps / usefull tools
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get update -y -q \
    && apt-get upgrade -y -q \
    && apt-get install --no-install-recommends -y -q \
        ## sometimes needed for doing crypto stuff
        # ca-certificates \
        ## for downloading and testing web/api
        wget \
        curl \
        ## an easy to use editor
        nano \
        ## git (thought it might not be usefull without explicitly
        ## mapping local configs and keys into the container)
        # git \
    ## cleanup
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/* \
    ## setup npm to place cache under moint point (easy reuse in dev)
    && npm set cache ${NPM_CACHE}

###
# Add node User and sudo config
###

ARG USERNAME=node
ARG USER_UID=1000
ARG USER_GID=1000

## add user group and user
RUN set -ex \
## node should be created by node js image
##    && groupadd --gid ${USER_GID} ${USERNAME} \
##    && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME} \
    && echo ${USERNAME} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${USERNAME} \
    && chmod 0440 /etc/sudoers.d/${USERNAME}

## Mountpoint for App Source & Workdir
VOLUME /app
WORKDIR /app

## switch to node user
USER node

## run install && build
CMD npm install \
    && npm run build

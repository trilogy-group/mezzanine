FROM ubuntu:18.04

LABEL description="Devspaces implementation for Mezzanine SCM"

# Install deps
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get -y --allow-downgrades --allow-remove-essential --allow-change-held-packages install \
    apt-transport-https \
    curl \
    htop \
    mc \
    make \
    build-essential \
    socat \
    wget \
    vim  && \
    rm -rf /var/lib/apt/lists/*

#
# UTF-8 by default
#
RUN apt-get -qq update && \
    apt-get -qqy install gnupg2 locales && \
    locale-gen en_US.UTF-8 && \
    rm -rf /var/lib/apt/lists/*

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

#
# Install python dev env
#
RUN \
    apt-get update -y && \
    apt-get install -y python-pip python-dev && \
    python -m pip install --upgrade pip setuptools && \
    rm -rf /var/lib/apt/lists/*

#
# Install Django
#
RUN \
    python -m pip install django

#
# Install Image scripts
#
COPY assets/entrypoint.sh /opt/scripts/entrypoint.sh
RUN chmod +x /opt/scripts/entrypoint.sh

WORKDIR /data

EXPOSE 8000

ENTRYPOINT /opt/scripts/entrypoint.sh

CMD tail -f /dev/null

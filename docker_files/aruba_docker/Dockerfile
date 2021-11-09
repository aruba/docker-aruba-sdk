FROM alpine:latest

# Install Python3 and pip3
# This hack is widely applied to avoid python printing issues in docker containers.
# See: https://github.com/Docker-Hub-frolvlad/docker-alpine-python3/pull/13
ENV PYTHONUNBUFFERED=1

RUN echo "**** install Python ****" && \
    apk update && \
    apk add --no-cache python3 && \
    echo "**** install pip ****" && \
    apk add --no-cache py3-pip && \
    pip3 install --no-cache --upgrade pip setuptools wheel && \
    echo "**** install Ansible ****" && \
    apk add --no-cache ansible && \
    echo "**** install bash, telnet and ssh ****" && \
    apk add --no-cache bash && \
    apk add --no-cache busybox-extras && \
    apk add --no-cache openssh && \
    rm -rf /tmp/* && \
    rm -rf /var/cache/apk/*

# Install pycentral and pyaoscx SDK
RUN echo "**** install aruba python packages ****" && \
    pip3 install pycentral && \
    pip3 install pyaoscx

# Install galaxy roles for Aruba
RUN echo "**** install aruba ansible galaxy roles and collections ****" && \
    ansible-galaxy install arubanetworks.aos_wlan_role && \
    ansible-galaxy install arubanetworks.aruba_central_role && \
    ansible-galaxy install arubanetworks.aoscx_role && \
    ansible-galaxy collection install arubanetworks.aos_switch

RUN echo "**** create directories for sample scripts  ****" && \
    mkdir /aruba && \
    mkdir /aruba/python && \
    mkdir /aruba/ansible && \
    mkdir /aruba/python/central && \
    mkdir /aruba/ansible/central

COPY samples/python_central/sample_scripts /aruba/python/central
COPY samples/ansible_aos/sample_playbooks /aruba/ansible/central

WORKDIR /aruba
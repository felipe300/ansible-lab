FROM ubuntu:24.04

LABEL maintainer="Felipe Gutierrez"
LABEL role="ansible-base"

ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /lab

RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 \
    python3-pip \
    python3-venv \
    sudo \
    curl \
    unzip \
    iputils-ping \
    vim \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

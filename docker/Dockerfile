FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /lab

RUN apt-get update && apt-get install -y \
    openssh-server \
    python3 \
    python3-pip \
    sudo \
    nginx \
    curl \
    unzip \
    iputils-ping \
    vim \
    && rm -rf /var/lib/apt/lists/*

# AWS CLI install
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip \
    && ./aws/install \
    && rm -rf awscliv2.zip aws/

# SSH setup + ansible user
RUN mkdir /var/run/sshd \
  && useradd -m -s /bin/bash ansible \
  && echo "ansible:ansible" | chpasswd \
  && echo "ansible ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# nginx logs redirect
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log

# Expose ports SSH y HTTP
EXPOSE 22 80

CMD ["/usr/sbin/sshd", "-D"]

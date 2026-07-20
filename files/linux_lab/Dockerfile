FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt update && apt install -y \
    openssh-server \
    sudo \
    vim \
    nano \
    curl \
    wget \
    tree \
    iproute2 \
    net-tools \
    procps \
    less \
    grep \
    findutils \
    && rm -rf /var/lib/apt/lists/*


# Create SSH directory
RUN mkdir /var/run/sshd


# Create student user FIRST
RUN useradd -m -s /bin/bash student \
    && echo "student:oslab123" | chpasswd \
    && usermod -aG sudo student


# Passwordless sudo
RUN echo "student ALL=(ALL) NOPASSWD:ALL" \
    > /etc/sudoers.d/student


# Copy CTF content
COPY challenge /home/student/challenge
COPY distractions /home/student/


# Fix ownership AFTER user exists
RUN chown -R student:student /home/student


# SSH configuration
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config


EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]

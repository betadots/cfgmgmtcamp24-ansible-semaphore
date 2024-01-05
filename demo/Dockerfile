FROM ubuntu:22.04
LABEL maintainer="Robert Waffen <https://github.com/rwaffen>"

RUN apt update && \
    apt install -y --no-install-recommends openssh-server libssl-dev sudo && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir -p /var/run/sshd /root/.ssh /home/ansible/.ssh

COPY ansible.pub /root/.ssh/authorized_keys
COPY ansible.pub /home/ansible/.ssh/authorized_keys

### add demo user
RUN useradd -m ansible -s /bin/bash && \
    chmod 700 /home/ansible/.ssh && \
    chmod 700 /root/.ssh && \
    chown ansible:ansible /home/ansible/.ssh && \
    echo "ansible ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/ansible && \
    chmod 0440 /etc/sudoers.d/ansible && \
    chmod 600 /home/ansible/.ssh/authorized_keys && \
    chmod 600 /root/.ssh/authorized_keys && \
    chown ansible:ansible /home/ansible/.ssh/authorized_keys

RUN echo "PermitRootLogin yes"      >> /etc/ssh/sshd_config.d/permit_root.conf && \
    echo "PubkeyAuthentication yes" >> /etc/ssh/sshd_config.d/pubkey_auth.conf && \
    echo "LogLevel VERBOSE"         >> /etc/ssh/sshd_config.d/log_level.conf

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D", "-e"]

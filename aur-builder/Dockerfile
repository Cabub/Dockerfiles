FROM archlinux:latest

ARG USER_UID=1000
ARG USER_GID=1000

RUN pacman-key --init && \
    curl -s http://jarvis.lab-1.cabub.me:8081/jarvis-public-key.asc | pacman-key --add - && \
    pacman-key --lsign-key jarvis@cabub.me && \
    cat >> /etc/pacman.conf <<'EOF'

[cabub.me]
SigLevel = Required
Server = http://jarvis.lab-1.cabub.me:8081/$arch
EOF

RUN pacman -Syu --noconfirm && \
    pacman -S --noconfirm base-devel git sudo && \
    pacman -Scc --noconfirm

RUN userdel uuidd && \
    groupadd -g ${USER_GID} builder && \
    useradd -m -u ${USER_UID} -g ${USER_GID} builder && \
    echo 'builder ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

USER builder
WORKDIR /home/builder

CMD ["/bin/bash"]

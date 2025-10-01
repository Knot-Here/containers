FROM archlinux:latest

# Fast mirrors (optional, but can help)
# RUN sed -i '1s;^;Server = https://mirror.rackspace.com/archlinux/$repo/os/$arch\n;' /etc/pacman.d/mirrorlist

# Keyring refresh first to avoid signature issues
RUN pacman -Sy --noconfirm archlinux-keyring && pacman -Syu --noconfirm

# Core tooling + Neovim (0.11.x from Arch repos)
RUN pacman -S --noconfirm --needed \
    neovim git ripgrep fd base-devel \
    python python-pip nodejs npm tmux curl

# Create dev user that matches host (adjust UID/GID if your host isn’t 1000)
ARG UID=1000
ARG GID=1000
RUN groupadd -g ${GID} dev && useradd -m -u ${UID} -g ${GID} -s /bin/bash dev

WORKDIR /workspace
# Ensure /etc/machine-id exists to avoid warnings in minimal containers
RUN mkdir -p /run \
 && ( [ -s /run/machine-id ] || (cat /proc/sys/kernel/random/uuid | tr -d '-' > /run/machine-id) ) \
 && ln -sf /run/machine-id /etc/machine-id

USER dev

# Log nvim version at build time so you can confirm
RUN nvim --version | head -n 2


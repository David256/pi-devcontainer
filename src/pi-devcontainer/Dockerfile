FROM debian:trixie

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8

RUN apt-get update \
    && apt install -y --no-install-recommends \
    vim \
    socat \
    wget curl \
    bash bash-completion \
    tree \
    unzip \
    ripgrep \
    git \
    nodejs npm \
    openssh-client \
    ffmpeg \
    && apt-get -y clean \
    && rm -rf /var/lib/apt/lists/*

RUN groupadd -g 1000 vscode
RUN useradd -ms /bin/bash -u 1000 vscode -g vscode
RUN usermod -aG sudo vscode
RUN passwd -d vscode
RUN mkdir /workspaces
RUN chown -R vscode:vscode /workspaces
USER vscode

# Install npm only for this user
ENV NVM_DIR=/home/vscode/.nvm
RUN mkdir -p $NVM_DIR \
    && curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash \
    && . "$NVM_DIR/nvm.sh" \
    && nvm install 24 \
    && nvm alias default 24
ENV PATH="$NVM_DIR/versions/node/v24.15.0/bin:$PATH"

# Install bun only for this user
ENV BUN_INSTALL=/home/vscode/.bun
RUN mkdir -p $BUN_INSTALL \
    && curl -fsSL https://bun.com/install | bash
ENV PATH="$BUN_INSTALL/bin:$PATH"

# Install pi.dev
RUN "$BUN_INSTALL/bin/bun" add -g @earendil-works/pi-coding-agent

WORKDIR /workspaces

CMD ["sleep", "infinity"]


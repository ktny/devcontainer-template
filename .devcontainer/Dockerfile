FROM debian:bookworm-slim

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
        git \
        curl \
        ca-certificates \
        sudo \
        openssh-client \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME

RUN curl https://mise.jdx.dev/install.sh | sh \
    && echo 'eval "$(~/.local/bin/mise activate bash)"' >> ~/.bashrc

ENV PATH="/home/$USERNAME/.local/bin:$PATH"

WORKDIR /workspace
FROM debian:bookworm-slim

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ENV MISE_DATA_DIR="/usr/local/share/mise"
ENV PATH="/usr/local/share/mise/shims:${PATH}"
WORKDIR /workspace
SHELL ["/bin/bash", "-c"]

RUN apt update && export DEBIAN_FRONTEND=noninteractive \
    && apt install -y --no-install-recommends \
        git \
        curl \
        sudo \
        ca-certificates \
        openssh-client \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

RUN curl https://mise.run | MISE_INSTALL_PATH=/usr/local/bin/mise sh \
    && mise trust && mise install

# ENV PATH="/root/.local/share/mise/shims:${PATH}"
# ENV MISE_DATA_DIR="/root/.local/share/mise"

# RUN echo 'eval "$(/usr/local/bin/mise activate bash)"' >> ~/.bashrc

USER $USERNAME

FROM debian:bookworm-slim

WORKDIR /workspace

# install required packages
RUN apt update && export DEBIAN_FRONTEND=noninteractive \
    && apt install -y --no-install-recommends \
        git \
        curl \
        sudo \
        ca-certificates \
        openssh-client \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# create user, group & setup sudo permissions
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# install mise & tools
COPY .mise.toml .
ENV MISE_DATA_DIR="/usr/local/share/mise"
ENV PATH="/usr/local/share/mise/shims:${PATH}"
RUN curl https://mise.run | MISE_INSTALL_PATH=/usr/local/bin/mise sh \
    && mise trust && mise install

USER $USERNAME

FROM debian:bookworm-slim

WORKDIR /workspace

# Install required packages
RUN apt update && export DEBIAN_FRONTEND=noninteractive \
    && apt install -y --no-install-recommends \
        git \
        curl \
        sudo \
        ca-certificates \
        openssh-client \
        procps \
        aggregate \
        iptables \
        ipset \
        iproute2 \
        dnsutils \
        jq \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# Create user, group and setup sudo permissions
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Install mise and tools
COPY .mise.toml .
ENV MISE_DATA_DIR="/usr/local/share/mise"
ENV PATH="/usr/local/share/mise/shims:${PATH}"
RUN curl https://mise.run | MISE_INSTALL_PATH=/usr/local/bin/mise sh \
    && mise trust && mise install

# Copy and set up firewall script
COPY ./.devcontainer/init-firewall.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/init-firewall.sh
USER $USERNAME

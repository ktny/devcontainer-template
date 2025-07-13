FROM debian:bookworm-slim

WORKDIR /workspace

# Install required packages
RUN apt update && export DEBIAN_FRONTEND=noninteractive \
    && apt install -y --no-install-recommends \
        git \
        curl \
        wget \
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
        zsh \
        fzf \
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

# Persist bash history.
RUN SNIPPET="export PROMPT_COMMAND='history -a' && export HISTFILE=/commandhistory/.bash_history" \
    && mkdir /commandhistory \
    && touch /commandhistory/.bash_history \
    && chown -R $USERNAME /commandhistory

# Set the default shell to zsh rather than sh
ENV SHELL=/bin/zsh
RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.2.0/zsh-in-docker.sh)" -- \
  -t powerlevel10k/powerlevel10k \
  -p git \
  -p fzf \
  -a "source /usr/share/doc/fzf/examples/key-bindings.zsh" \
  -a "source /usr/share/doc/fzf/examples/completion.zsh" \
  -a "export PROMPT_COMMAND='history -a' && export HISTFILE=/commandhistory/.bash_history" \
  -x

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

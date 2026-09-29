# ==============================================================================
# Hermes Agent Pterodactyl Container
# Base Image: Python 3.11 with Playwright Chromium & Root Toolchain
# ==============================================================================

FROM ghcr.io/ptero-eggs/yolks:python_3.11

USER root

# Install system dependencies, network tools, runtime compilation, and Node.js
RUN apt-get update && apt-get install -y --no-install-recommends \
    sudo \
    curl \
    wget \
    jq \
    git \
    nmap \
    dnsutils \
    net-tools \
    iproute2 \
    procps \
    htop \
    tree \
    unzip \
    zip \
    tar \
    ripgrep \
    build-essential \
    default-jre-headless \
    nodejs \
    npm \
    libnss3 \
    libnspr4 \
    libatk1.0-0 \
    libatk-bridge2.0-0 \
    libcups2 \
    libdrm2 \
    libxkbcommon0 \
    libxcomposite1 \
    libxdamage1 \
    libxfixes3 \
    libxrandr2 \
    libgbm1 \
    libpango-1.0-0 \
    libcairo2 \
    libasound2 \
    && rm -rf /var/lib/apt/lists/*

# Setup passwordless sudo for container user (UID 998)
RUN echo "container ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/container && \
    chmod 0440 /etc/sudoers.d/container && \
    echo "ALL ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers.d/container

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

CMD ["/bin/bash"]

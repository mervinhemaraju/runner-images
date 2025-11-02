# # Use the official GitHub Actions runner as base
# FROM ghcr.io/actions/actions-runner:latest

# # Switch to root to install packages
# USER root

# # Combine layers and use --no-install-recommends to reduce image size
# RUN apt-get update && apt-get install -y --no-install-recommends \
#     curl \
#     wget \
#     jq \
#     git \
#     tar \
#     python3 \
#     python3-pip \
#     ca-certificates \
#     gnupg \
#     lsb-release \
#     && apt-get clean \
#     && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# # Create symlinks for python/pip directly (simpler and more reliable)
# RUN ln -sf /usr/bin/python3 /usr/bin/python && \
#     ln -sf /usr/bin/pip3 /usr/bin/pip

# # Create necessary directories
# RUN mkdir -p /home/runner/.oci/keys \
#     /home/runner/cloudflare \
#     /home/runner/.kube \
#     /home/runner/bin && \
#     chown runner:runner /home/runner/cloudflare && \
#     chown runner:runner /home/runner/.oci && \
#     chown runner:runner /home/runner/.oci/keys && \
#     chown runner:runner /home/runner/.kube && \
#     chown runner:runner /home/runner/bin

# # Switch to runner user BEFORE installing OCI CLI
# USER runner

# # Install OCI CLI as runner user
# RUN curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh > /tmp/install-oci-cli.sh && \
#     chmod +x /tmp/install-oci-cli.sh && \
#     /tmp/install-oci-cli.sh --accept-all-defaults && \
#     rm -f /tmp/install-oci-cli.sh

# # Add to PATH and also update runner's bash profile
# RUN echo 'export PATH="/home/runner/bin:$PATH"' >> /home/runner/.bashrc && \
#     echo 'export PATH="/home/runner/bin:$PATH"' >> /home/runner/.profile

# ENV PATH="/home/runner/bin:${PATH}"

# # Set working directory
# WORKDIR /home/runner

# # Default command
# CMD ["/home/runner/run.sh"]



# Use the official GitHub Actions runner as base
FROM ghcr.io/actions/actions-runner:latest

# Switch to root to install packages
USER root

# Combine layers and use --no-install-recommends to reduce image size
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    wget \
    jq \
    git \
    tar \
    python3 \
    python3-pip \
    ca-certificates \
    gnupg \
    lsb-release \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install Docker
RUN install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    chmod a+r /etc/apt/keyrings/docker.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
    docker-ce \
    docker-ce-cli \
    containerd.io \
    docker-buildx-plugin \
    docker-compose-plugin && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Add runner user to docker group
RUN usermod -aG docker runner

# Create symlinks for python/pip directly (simpler and more reliable)
RUN ln -sf /usr/bin/python3 /usr/bin/python && \
    ln -sf /usr/bin/pip3 /usr/bin/pip

# Create necessary directories
RUN mkdir -p /home/runner/.oci/keys \
    /home/runner/cloudflare \
    /home/runner/.kube \
    /home/runner/bin && \
    chown runner:runner /home/runner/cloudflare && \
    chown runner:runner /home/runner/.oci && \
    chown runner:runner /home/runner/.oci/keys && \
    chown runner:runner /home/runner/.kube && \
    chown runner:runner /home/runner/bin

# Switch to runner user BEFORE installing OCI CLI
USER runner

# Install OCI CLI as runner user
RUN curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh > /tmp/install-oci-cli.sh && \
    chmod +x /tmp/install-oci-cli.sh && \
    /tmp/install-oci-cli.sh --accept-all-defaults && \
    rm -f /tmp/install-oci-cli.sh

# Add to PATH and also update runner's bash profile
RUN echo 'export PATH="/home/runner/bin:$PATH"' >> /home/runner/.bashrc && \
    echo 'export PATH="/home/runner/bin:$PATH"' >> /home/runner/.profile

ENV PATH="/home/runner/bin:${PATH}"

# Set working directory
WORKDIR /home/runner

# Default command
CMD ["/home/runner/run.sh"]
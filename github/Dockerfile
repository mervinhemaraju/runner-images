# Use the official GitHub Actions runner as base
FROM ghcr.io/actions/actions-runner:latest

# Switch to root to install packages
USER root

# Combine layers and use --no-install-recommends to reduce image size
# Use apt-get clean and remove cache in the same layer
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

# Install OCI CLI - cache this layer separately as it changes less frequently
RUN curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh > /tmp/install-oci-cli.sh && \
    chmod +x /tmp/install-oci-cli.sh && \
    /tmp/install-oci-cli.sh --accept-all-defaults && \
    rm -f /tmp/install-oci-cli.sh

# Create symlinks for python/pip (this works system-wide)
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1 && \
    update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1

# Create necessary directories in one layer
RUN mkdir -p /home/runner/.oci/keys \
    /home/runner/cloudflare \
    /home/runner/.kube && \
    chown -R runner:runner /home/runner/.oci \
    /home/runner/cloudflare \
    /home/runner/.kube

# Add OCI CLI to PATH for all users
ENV PATH="/root/bin:/home/runner/bin:${PATH}"

# Switch back to runner user
USER runner

# Set working directory
WORKDIR /home/runner

# Default command
CMD ["/home/runner/run.sh"]
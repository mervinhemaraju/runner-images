# Use the official GitHub Actions runner as base
FROM ghcr.io/actions/actions-runner:latest

# Switch to root to install packages
USER root

# Install required packages
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    jq \
    git \
    tar \
    python3 \
    python3-pip \
    # postgresql-client \
    # mariadb-client \
    ca-certificates \
    gnupg \
    lsb-release \
    && rm -rf /var/lib/apt/lists/*

# Install Docker CLI (for Docker-in-Docker scenarios)
# RUN install -m 0755 -d /etc/apt/keyrings && \
#     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
#     chmod a+r /etc/apt/keyrings/docker.gpg && \
#     echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
#     $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
#     apt-get update && \
#     apt-get install -y docker-ce-cli && \
#     rm -rf /var/lib/apt/lists/*

# Install OCI CLI
RUN curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh > /tmp/install-oci-cli.sh && \
    chmod +x /tmp/install-oci-cli.sh && \
    /tmp/install-oci-cli.sh --accept-all-defaults && \
    rm /tmp/install-oci-cli.sh

# Install Certbot and Cloudflare plugin
# RUN pip3 install --no-cache-dir certbot certbot-dns-cloudflare

# Set up Python aliases
RUN echo 'alias python=python3' >> /etc/bash.bashrc && \
    echo 'alias pip=pip3' >> /etc/bash.bashrc

# Create necessary directories
RUN mkdir -p /home/runner/.oci/keys && \
    mkdir -p /home/runner/cloudflare && \
    mkdir -p /home/runner/.kube && \
    chown -R runner:runner /home/runner/.oci && \
    chown -R runner:runner /home/runner/cloudflare && \
    chown -R runner:runner /home/runner/.kube

# Add OCI CLI to PATH
ENV PATH="/root/bin:${PATH}"

# Switch back to runner user
USER runner

# Set working directory
WORKDIR /home/runner

# Default command (will be overridden by the runner startup script)
CMD ["/home/runner/run.sh"]
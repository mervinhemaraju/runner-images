# Base Image
FROM public.ecr.aws/spacelift/runner-terraform:latest

# Set Environment variables
ENV PATH="${PATH}:/home/spacelift/.local/bin:/usr/local/.pyenv/bin"

# Labels
LABEL "maintainer"="Mervin Hemaraju <mervinhemaraju16@gmail.com>"
LABEL "application"="mervinhemaraju.app.spacelift.io"

# Change user to root
USER root

# Specify python version during image build
ARG PYTHON_VERSION=3.12

# Install Python from Alpine repositories (much faster!)
RUN apk add --no-cache \
    python3~=${PYTHON_VERSION} \
    py3-pip \
    wget

# Set pip3 as the default pip
RUN ln -sf /usr/bin/pip3 /usr/bin/pip

# Change Work directory
WORKDIR /tmp

# Ensure we're using 'spacelift' for actually running the Spacelift Jobs
USER spacelift
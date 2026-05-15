FROM mcr.microsoft.com/vscode/devcontainers/base:debian@sha256:e389149057371298eaeb72e736fc2c4dcb79974de222dd471d2e6d675d2f61c4

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install system dependencies required by Home Assistant
RUN \
    apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get upgrade -y \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        bluez \
        ffmpeg \
        libudev-dev \
        libavformat-dev \
        libavcodec-dev \
        libavdevice-dev \
        libavutil-dev \
        libswscale-dev \
        libswresample-dev \
        libavfilter-dev \
        libpcap-dev \
        libturbojpeg0 \
        libyaml-dev \
        libxml2 \
        git \
        cmake \
        autoconf \
        pkg-config \
        libffi-dev \
        libssl-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Add go2rtc binary for WebRTC support
COPY --from=ghcr.io/alexxit/go2rtc@sha256:675c318b23c06fd862a61d262240c9a63436b4050d177ffc68a32710d9e05bae /usr/local/bin/go2rtc /bin/go2rtc

# Copy uv (Python package manager) for fast dependency resolution
COPY --from=ghcr.io/astral-sh/uv@sha256:1025398289b62de8269e70c45b91ffa37c373f38118d7da036fb8bb8efc85d97 /uv /usr/local/bin/uv

WORKDIR /usr/src

USER vscode

# Set up Python virtual environment
ENV VIRTUAL_ENV="/home/vscode/.local/ha-venv"
ENV PYTHON_VERSION="3.13.11"

RUN uv python install ${PYTHON_VERSION} \
    && uv venv ${VIRTUAL_ENV}

ENV PATH="${VIRTUAL_ENV}/bin:${PATH}"

# Clone Home Assistant Core for development
# This provides type hints, completions, and ability to debug into HA Core
WORKDIR /workspaces/core
RUN git clone --branch 2024.12.5 --depth 1 https://github.com/home-assistant/core.git .

# Create config directory structure for custom component
RUN mkdir -p /workspaces/core/config/custom_components

WORKDIR /workspaces

# Set environment variables
ENV SHELL=/bin/bash \
    PYTHONASYNCIODEBUG=1 \
    CONTAINER_WORKSPACE_FOLDER="/workspaces/ha-openaicust"

# Default command
CMD ["/bin/bash"]

# CMap sigtool-runtime
# This Dockerfile configures an runtime environment for executing
# command-line implementations of core Connectivity Map algorithms (sig-tools)
# Build using:
# docker build --no-cache=true --build-arg BUILD_DATE=$(date -u +'%Y-%m-%dT%H:%M:%SZ') --build-arg BUILD_VERSION="test" -t sigtool-runtime:latest .

# A Docker optimized mini Debian image
FROM bitnami/minideb:buster

ARG BUILD_DATE
ARG BUILD_VERSION="latest"
# Matlab compiler runtime release
ARG MCR_RELEASE='R2014b'
# MCR version number
ARG MCR_VERSION_NUM='v84'

#R

ARG VOLUME_PATH="/opt"

# Install location of MCR
ARG MCR_INSTALL_PATH=${VOLUME_PATH}/mcr

# Location of MCR files
ENV MCR_ROOT="${MCR_INSTALL_PATH}/${MCR_VERSION_NUM}"

# URL of MCR Installer
ARG MCR_URL="https://ssd.mathworks.com/supportfiles/downloads/${MCR_RELEASE}/deployment_files/${MCR_RELEASE}/installers/glnxa64/MCR_${MCR_RELEASE}_glnxa64_installer.zip"

ENV DEBIAN_FRONTEND noninteractive

LABEL org.label-schema.name="sigtool-runtime" \
  org.label-schema.description="Provides a containerized runtime environment to execute CMap sig-tools" \
  org.label-schema.version="$BUILD_VERSION" \
  org.label-schema.build-date="$BUILD_DATE" \
  org.label-schema.license="GPL-2.0" \
  org.label-schema.vcs-url="https://github.com/cmap/cmap-sig-tools" \
  org.label-schema.schema-version="1.0"

# Copy scripts
COPY ./assets/* /usr/local/bin

RUN install_packages \
  wget \
  tar \
  unzip \
  libxt-dev \
  libxtst6 \
  libncurses5 \
  libxext6 \
  libxmu6 \
  libxpm-dev \
  libxt6 \
  bc \
  gnuplot \
  vim-tiny \
  procps \
&& mkdir -p /tmp/mcr_installer $MCR_INSTALL_PATH \
## Download MCR
&& cd /tmp/mcr_installer \
&& echo "Downloading MCR from: $MCR_URL" \
&& wget --no-verbose --no-check-certificate -S "$MCR_URL" -O mcr_installer.zip \
&& unzip -q mcr_installer.zip \
## Install MCR
&& ./install -mode silent -agreeToLicense Yes -destinationFolder "$MCR_INSTALL_PATH" \
## set permissions to sig-tool scripts
chmod +x /usr/local/{run-sigtool,bashtk} \
## Cleanup
&& cd / \
&& rm -rf /tmp/* \
&& apt-get autoremove -y \
&& apt-get autoclean -y \
&& rm -rf /var/lib/apt/lists/* \
## Remove extraneous MCR files
&& rm -rf /opt/mcr/v84/sys/jxbrowser-chromium \
&& find  /opt/mcr/ -name 'ja_JP' -type d -prune -exec rm -r {} \;

# Configure library paths for MCR
# Note that setting LD_LIBRARY_PATH can break other apps so as a workaround
# save paths to alternate variable instead and use as:
# LD_LIBRARY_PATH=$MCR_LIBRARY_PATH <matlab_program> args
ENV MCR_LD_LIBRARY_PATH "${MCR_ROOT}/runtime/glnxa64:${MCR_ROOT}/bin/glnxa64:${MCR_ROOT}/sys/os/glnxa64"
ENV XAPPLRESDIR ${MCR_ROOT}/X11/app-defaults

VOLUME ["$VOLUME_PATH"]

CMD ["bash"]

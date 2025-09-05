ARG BUILD_FROM=python:3.11-alpine
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Build architecture
ARG BUILD_ARCH=aarch64
ENV BUILD_ARCHI=${BUILD_ARCH}

# Other settings
ENV LANG C.UTF-8
ENV DIYHUE_VERSION=master
ENV DIYHUE_COMMIT=9604db9241605a2f3ae4b5504ce3c4c3ea29ec6f
ENV DIYHUE_UI_VERSION=v2.0.3
ENV WORKING_DIR=/opt/hue-emulator

# Create folders
RUN mkdir -p diyhue config ${WORKING_DIR}

# Install dependencies
RUN apk add -q -u python3 openssl nmap psmisc iproute2 alpine-sdk build-base \
    && apk add --no-cache bluez bash curl unzip git make musl-dev gcc

# Upgrade pip
RUN python -m pip install --upgrade pip

# Download diyHue and unzip
RUN curl -sL -o diyhue.zip https://github.com/babadam2002/diyHue/archive/${DIYHUE_COMMIT}.zip
RUN unzip -qo diyhue.zip
RUN rm diyhue.zip
RUN pip install -r diyHue-${DIYHUE_COMMIT}/requirements.txt --no-cache-dir

# Copy diyHue files
RUN cp -r diyHue-${DIYHUE_COMMIT}/BridgeEmulator/* ${WORKING_DIR}/

# Download and unzip UI
RUN curl -sL -o diyHueUI.zip https://github.com/diyhue/diyHueUI/releases/download/${DIYHUE_UI_VERSION}/DiyHueUI-release.zip
RUN mkdir -p /diyhueUI
RUN unzip -qo diyHueUI.zip -d /diyhueUI
RUN rm diyHueUI.zip

# Ensure templates directory exists
RUN mkdir -p ${WORKING_DIR}/flaskUI/templates

# Copy UI files
RUN cp /diyhueUI/dist/index.html ${WORKING_DIR}/flaskUI/templates/index.html
RUN cp -r /diyhueUI/dist/assets ${WORKING_DIR}/flaskUI/
RUN rm -r /diyhueUI

# Clean up diyHue source
RUN rm -r diyHue-${DIYHUE_COMMIT}

# Optional: install libfaketime
RUN apk --no-cache add --virtual build-deps gcc git make musl-dev \
    && git clone https://github.com/wolfcw/libfaketime \
    && cd libfaketime && git checkout v0.9.10 && make \
    && mv src/*.so* /lib && mv src/faketime /bin \
    && cd - && rm -rf libfaketime \
    && apk del build-deps

# Set working directory
WORKDIR ${WORKING_DIR}

# Copy additional local build files
COPY rootfs ./

# Modify permissions
RUN chmod +x ./select.sh ./genCert.sh && chmod a+x ./run.sh

# Select the right binaries depending on architecture
RUN ./select.sh

# Build arguments
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Document volume
VOLUME ["/config"]

# Labels
LABEL \
    io.hass.name="diyHue" \
    io.hass.description="Fully configurable diyHue Emulator" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="diyHue <info@diyHue.org>" \
    org.opencontainers.image.title="diyHue" \
    org.opencontainers.image.description="Fully configurable diyHue Emulator" \
    org.opencontainers.image.vendor="diyHue" \
    org.opencontainers.image.authors="diyHue <info@diyHue.org>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="diyHue.org" \
    org.opencontainers.image.source="https://github.com/diyhue/diyhue" \
    org.opencontainers.image.documentation="diyhue.readthedocs.io" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}

CMD ["./run.sh"]


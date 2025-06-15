ARG BUILD_FROM=hassioaddons/base-python:5.3.4
FROM ${BUILD_FROM}

# Shell beállítás
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ARG BUILD_ARCH=aarch64
ENV BUILD_ARCHI=${BUILD_ARCH}

ENV LANG=C.UTF-8
ENV DIYHUE_VERSION=master
ENV DIYHUE_COMMIT=master
ENV WORKING_DIR=/opt/hue-emulator

# Frissítsük az apk indexet, telepítsük a függőségeket
RUN apk update && apk add --no-cache \
    python3 \
    py3-pip \
    gcc \
    musl-dev \
    libffi-dev \
    openssl-dev \
    make \
    bzip2 \
    wget \
    curl \
    unzip \
    nmap \
    psmisc \
    iproute2 \
    bluez \
    alpine-sdk \
    build-base

# Készítsük el a mappákat
RUN mkdir -p diyhue config ${WORKING_DIR}

# Letöltés, telepítés, másolás stb. (eredeti scripted alapján)
RUN curl -sL -o diyhue.zip https://github.com/diyhue/diyHue/archive/${DIYHUE_COMMIT}.zip && \
    unzip -qo diyhue.zip && \
    rm diyhue.zip && \
    pip3 install -r /diyHue-${DIYHUE_COMMIT}/requirements.txt --no-cache-dir && \
    cp -r /diyHue-${DIYHUE_COMMIT}/BridgeEmulator/* ${WORKING_DIR}/ && \
    curl -sL -o diyHueUI.zip https://www.github.com/diyhue/diyHueUI/releases/latest/download/DiyHueUI-release.zip && \
    mkdir /diyhueUI && \
    unzip -qo diyHueUI.zip -d /diyhueUI && \
    rm diyHueUI.zip && \
    cp -r /diyhueUI/dist/index.html ${WORKING_DIR}/flaskUI/templates/ && \
    cp -r /diyhueUI/dist/assets ${WORKING_DIR}/flaskUI/ && \
    rm -rf /diyhueUI && \
    rm -rf /diyHue-${DIYHUE_COMMIT}

# libfaketime telepítése
RUN apk --no-cache add --virtual build-deps git gcc make musl-dev && \
    git clone https://github.com/wolfcw/libfaketime && \
    cd libfaketime && \
    git checkout v0.9.10 && \
    make && \
    mv src/*.so* /lib && mv src/faketime /bin && \
    cd .. && \
    rm -rf libfaketime && \
    apk del build-deps

WORKDIR ${WORKING_DIR}

COPY rootfs ./

RUN chmod +x ./select.sh ./genCert.sh ./run.sh

RUN ./select.sh

ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

VOLUME ["/config"]

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

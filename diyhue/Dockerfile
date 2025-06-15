ARG BUILD_FROM=python:3.11-alpine
FROM ${BUILD_FROM}

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ARG BUILD_ARCH=aarch64
ENV BUILD_ARCH=${BUILD_ARCH}
ENV LANG C.UTF-8
ENV DIYHUE_VERSION=master
ENV WORKING_DIR=/opt/hue-emulator

WORKDIR /opt/hue-emulator

RUN mkdir -p diyhue config ${WORKING_DIR} && \
    apk add --no-cache python3 py3-pip openssl nmap psmisc iproute2 alpine-sdk build-base bluez curl unzip && \
    pip3 install --upgrade pip

RUN curl -sL -o diyhue.zip https://github.com/diyhue/diyHue/archive/master.zip && \
    unzip -qo diyhue.zip && rm diyhue.zip && \
    pip3 install -r /diyHue-master/requirements.txt --no-cache-dir

# Másolás és jogok beállítása

RUN cp -r /diyHue-master/BridgeEmulator/* /opt/hue-emulator/ && \
    curl -sL -o diyHueUI.zip https://www.github.com/diyhue/diyHueUI/releases/latest/download/DiyHueUI-release.zip && \
    mkdir /diyhueUI && unzip -qo diyHueUI.zip -d /diyhueUI && rm diyHueUI.zip && \
    cp -r /diyhueUI/dist/index.html /opt/hue-emulator/flaskUI/templates/ && \
    cp -r /diyhueUI/dist/assets /opt/hue-emulator/flaskUI/ && \
    rm -r /diyhueUI && \
    rm -r /diyHue-master

RUN apk --no-cache add --virtual build-deps gcc git make musl-dev && \
    git clone https://github.com/wolfcw/libfaketime && \
    cd libfaketime && git checkout v0.9.10 && make && \
    mv src/*.so* /lib && mv src/faketime /bin && cd - && \
    rm -rf libfaketime && apk del build-deps

COPY rootfs ./

RUN chmod +x ./select.sh ./genCert.sh ./run.sh && ./select.sh

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

CMD [ "./run.sh" ]

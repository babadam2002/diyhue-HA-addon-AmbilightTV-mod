ARG BUILD_FROM=hassioaddons/base-python:5.3.4
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Build architecture
ARG BUILD_ARCH=aarch64
ENV BUILD_ARCHI=${BUILD_ARCH}

# Other settings
ENV LANG C.UTF-8
ENV DIYHUE_VERSION=master
ENV DIYHUE_COMMIT=master
ENV WORKING_DIR=/opt/hue-emulator

# Create needed folders
RUN mkdir diyhue config ${WORKING_DIR}

# Telepítjük az alap build eszközöket és függőségeket Python buildhez
RUN apk add --no-cache --virtual .build-deps \
    gcc musl-dev libffi-dev openssl-dev make bzip2 wget curl unzip alpine-sdk build-base \
    python3-dev py3-pip \
    nmap psmisc iproute2 bluez

# Letöltjük és buildeljük a Python 3.11.5 verziót (vagy ahogy szeretnéd)
RUN wget https://www.python.org/ftp/python/3.11.5/Python-3.11.5.tgz && \
    tar xzf Python-3.11.5.tgz && \
    cd Python-3.11.5 && \
    ./configure --enable-optimizations && \
    make && make install && \
    cd .. && \
    rm -rf Python-3.11.5 Python-3.11.5.tgz

# Eltávolítjuk a build függőségeket, hogy kisebb legyen az image
RUN apk del .build-deps

# Letöltjük a diyHue forrást és feltelepítjük a függőségeket
RUN curl -sL -o diyhue.zip https://github.com/diyhue/diyHue/archive/${DIYHUE_COMMIT}.zip && \
    unzip -qo diyhue.zip && \
    rm diyhue.zip && \
    pip3 install -r /diyHue-${DIYHUE_COMMIT}/requirements.txt --no-cache-dir

# Másoljuk a diyHue fájlokat
RUN cp -r /diyHue-${DIYHUE_COMMIT}/BridgeEmulator/flaskUI /opt/hue-emulator/ && \
    cp -r /diyHue-${DIYHUE_COMMIT}/BridgeEmulator/functions /opt/hue-emulator/ && \
    cp -r /diyHue-${DIYHUE_COMMIT}/BridgeEmulator/lights /opt/hue-emulator/ && \
    cp -r /diyHue-${DIYHUE_COMMIT}/BridgeEmulator/sensors /opt/hue-emulator/ && \
    cp -r /diyHue-${DIYHUE_COMMIT}/BridgeEmulator/HueObjects /opt/hue-emulator/ && \
    cp -r /diyHue-${DIYHUE_COMMIT}/BridgeEmulator/services /opt/hue-emulator/ && \
    cp -r /diyHue-${DIYHUE_COMMIT}/BridgeEmulator/configManager /opt/hue-emulator/ && \
    cp -r /diyHue-${DIYHUE_COMMIT}/BridgeEmulator/logManager /opt/hue-emulator/ && \
    cp -r /diyHue-${DIYHUE_COMMIT}/BridgeEmulator/HueEmulator3.py /opt/hue-emulator/ && \
    cp -r /diyHue-${DIYHUE_COMMIT}/BridgeEmulator/githubInstall.sh /opt/hue-emulator/ && \
    cp -r /diyHue-${DIYHUE_COMMIT}/BridgeEmulator/genCert.sh /diyHue-${DIYHUE_COMMIT}/BridgeEmulator/openssl.conf /opt/hue-emulator/

# Letöltjük és telepítjük a diyHueUI legfrissebb verzióját
RUN curl -sL -o diyHueUI.zip https://www.github.com/diyhue/diyHueUI/releases/latest/download/DiyHueUI-release.zip && \
    mkdir /diyhueUI && \
    unzip -qo diyHueUI.zip -d /diyhueUI && \
    rm diyHueUI.zip && \
    cp -r /diyhueUI/dist/index.html /opt/hue-emulator/flaskUI/templates/ && \
    cp -r /diyhueUI/dist/assets /opt/hue-emulator/flaskUI/ && \
    rm -r /diyhueUI

# Töröljük a forráskódot
RUN rm -r /diyHue-${DIYHUE_COMMIT}

# Telepítjük a libfaketime-t a szükséges verzióval
RUN apk --no-cache add --virtual build-deps gcc git make musl-dev && \
    git clone https://github.com/wolfcw/libfaketime && \
    cd libfaketime && \
    git checkout v0.9.10 && \
    make && \
    mv src/*.so* /lib && mv src/faketime /bin && \
    cd .. && rm -rf libfaketime && \
    apk del build-deps

# Beállítjuk a munkakönyvtárat
WORKDIR /opt/hue-emulator

# Másoljuk az addonhoz tartozó extra fájlokat
COPY rootfs ./

# Jogok módosítása
RUN chmod +x ./select.sh ./genCert.sh && chmod a+x ./run.sh

# Válasszuk ki az architektúrának megfelelő binárisokat
RUN ./select.sh

# Dokumentálás
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

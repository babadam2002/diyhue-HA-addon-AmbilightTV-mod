ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base-python:3.12
FROM $BUILD_FROM

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV LANG C.UTF-8
ENV WORKING_DIR=/opt/hue-emulator

# Install dependencies
RUN apk add --no-cache git bash bluez nmap openssl iproute2

# Clone latest diyHue
RUN git clone https://github.com/diyhue/diyHue ${WORKING_DIR}

WORKDIR ${WORKING_DIR}

# Install Python deps
RUN pip install --no-cache-dir -r requirements.txt

# Copy addon run script
COPY rootfs/ /

RUN chmod +x /run.sh

CMD [ "/run.sh" ]

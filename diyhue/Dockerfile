ARG BUILD_FROM=ghcr.io/hassio-addons/base-python:3.12
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
COPY run.sh /run.sh
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]

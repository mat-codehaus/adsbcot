# Download base image ubuntu 22.04
FROM --platform=$TARGETPLATFORM ubuntu:22.04

# LABEL about the custom image
LABEL description="ADS-B to TAK Gateway."

# Disable Prompt During Packages Installation
ARG DEBIAN_FRONTEND=noninteractive

# Update Ubuntu Software repository and install Python modules
RUN apt update && apt upgrade -y && apt install -y \
    python3 \
    python3-pip \
    python3-numpy \
    python3-zmq \
    python3-cryptography \
    rtl-sdr && \
    python3 -m pip install asyncinotify pymodes pyrtlsdr && \
    apt clean && rm -rf /var/lib/apt/lists/*

# Build adsbcot
COPY . /adsbcot/
RUN cd /adsbcot/ && python3 setup.py install easy_install 'adsbcot[with_pymodes]'
RUN rm -R /adsbcot/

# Copy adsbcot configuration
RUN mkdir /etc/adsbcot/
COPY ./docker/adsbcot-docker-example.conf /etc/adsbcot/adsbcot.conf

# Copy start.sh script and define default command for the container
COPY ./docker/start.sh /start.sh
CMD ["./start.sh"]

# Dockerfile.gaianet1
FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y \
        curl \
        tar \
        git \
        openssl \
        wget \
        python3 \
        python3-pip \
        build-essential \
        libssl-dev \
        pciutils && \
    rm -rf /var/lib/apt/lists/*

RUN useradd -m gaianet1 && \
    mkdir -p /home/gaianet1/gaianet1 && \
    chown -R gaianet1:gaianet1 /home/gaianet1

USER gaianet1
WORKDIR /home/gaianet1

ENV HOME=/home/gaianet1
ENV GAIANET_BASE_DIR=/home/gaianet1/gaianet1
ENV PATH="/home/gaianet1/gaianet1/bin:${PATH}"

RUN mkdir -p $GAIANET_BASE_DIR/{bin,log,qdrant,gaia-frp,tmp}

RUN for i in {1..5}; do \
      curl -sSfL 'https://github.com/GaiaNet-AI/gaianet-node/releases/latest/download/install.sh' | bash -s -- --base $GAIANET_BASE_DIR && break || \
      sleep 15; \
    done

RUN gaianet init --config https://raw.githubusercontent.com/PemburuSurya/gaianet/main/qwen1.5./config.json

COPY --chown=gaianet1:gaianet1 entrypoint.sh /home/gaianet1/entrypoint.sh
RUN chmod +x /home/gaianet1/entrypoint.sh

EXPOSE 8080 8081 8082 6333 6334

ENTRYPOINT ["/home/gaianet1/entrypoint.sh"]

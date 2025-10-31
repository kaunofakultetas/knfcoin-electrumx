FROM python:3.14.0-slim

WORKDIR /electrumx

# Install all dependencies (build + runtime) for leveldb
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential libleveldb-dev \
    && rm -rf /var/lib/apt/lists/*

# Copy the entire electrumx source tree
COPY . /electrumx

# Install from local source with rapidjson support
RUN pip install --upgrade pip \
    && pip install /electrumx[rapidjson]

# Data directory for electrumx
RUN mkdir -p /var/lib/electrumx

# Environment defaults
ENV SERVICES="tcp://:50001"
ENV COIN=Bitcoin
ENV NET=mainnet
ENV DB_DIRECTORY=/var/lib/electrumx
ENV DAEMON_URL="http://username:password@hostname:port/"
ENV ALLOW_ROOT=true
ENV DB_ENGINE=leveldb
ENV MAX_SEND=10000000
ENV BANDWIDTH_UNIT_COST=50000
ENV CACHE_MB=2000




######################################
########## KNFCOIN SPECIFIC ##########
######################################

# Install openssl for certificate generation (Debian/Ubuntu)
RUN apt-get update && apt-get install -y openssl && rm -rf /var/lib/apt/lists/*

# Create directory for SSL certificates
RUN mkdir -p /etc/electrumx/ssl

# Generate self-signed SSL certificate and key in /etc/electrumx/ssl
RUN openssl req -newkey rsa:2048 -sha256 -nodes \
    -keyout /etc/electrumx/ssl/server.key \
    -x509 -days 3650 \
    -out /etc/electrumx/ssl/server.crt \
    -subj "/CN=knfcoin-electrumx" && \
    chmod 644 /etc/electrumx/ssl/server.crt && \
    chmod 600 /etc/electrumx/ssl/server.key

######################################
######################################
######################################




CMD ["electrumx_server"]
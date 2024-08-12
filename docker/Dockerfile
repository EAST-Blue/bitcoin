FROM ubuntu:22.04

ARG UID=101
ARG GID=101

RUN apt update -y && apt install -y libevent-pthreads-2.1-7 libevent-2.1-7 libminiupnpc-dev libnatpmp-dev
RUN apt install -y libzmq3-dev libsqlite3-dev

RUN groupadd --gid ${GID} bitcoin \
  && useradd --create-home --no-log-init -u ${UID} -g ${GID} bitcoin \
  && apt-get update -y \
  && apt-get install -y gosu \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ARG TARGETPLATFORM
ENV BITCOIN_DATA=/home/bitcoin/.bitcoin
ENV PATH=/opt/bitcoin/bin:$PATH

COPY bitcoind /opt/bitcoin/bin/
COPY bitcoin-cli /opt/bitcoin/bin/
COPY bitcoin-tx /opt/bitcoin/bin/

COPY docker-entrypoint.sh /entrypoint.sh

VOLUME ["/home/bitcoin/.bitcoin"]

EXPOSE 8332 8333 18332 18333 18443 18444 38333 38332

ENTRYPOINT ["/entrypoint.sh"]

CMD ["bitcoind"]

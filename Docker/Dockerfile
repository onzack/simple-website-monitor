FROM ubuntu:20.04
LABEL maintainer="https://github.com/onzack"
RUN groupadd -g 10000 simple-user && useradd -u 10000 -g 10000 -s /bin/bash -m simple-user
COPY --chown=simple-user:simple-user entrypoint.sh /opt/entrypoint.sh
ENV TELEGRAF_VERSION 1.17.0
RUN chmod -x /opt/entrypoint.sh && \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends tzdata curl ca-certificates iputils-ping snmp procps lm-sensors && \
    curl -o telegraf_1.17.0-1_amd64.deb https://dl.influxdata.com/telegraf/releases/telegraf_${TELEGRAF_VERSION}-1_amd64.deb && \
    dpkg -i telegraf_${TELEGRAF_VERSION}-1_amd64.deb && \
    rm -f telegraf_${TELEGRAF_VERSION}-1_amd64.deb
    apt-get purge -y curl && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
COPY --chown=simple-user:simple-user entrypoint.sh /opt/entrypoint.sh
USER 10000
WORKDIR /home/simple-user
ENTRYPOINT ["/opt/opt/entrypoint.sh"]
CMD ["telegraf"]

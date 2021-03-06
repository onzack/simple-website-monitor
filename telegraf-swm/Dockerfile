FROM ubuntu:20.04
LABEL maintainer="https://github.com/onzack"
RUN groupadd -g 10000 simple-user && useradd -u 10000 -g 10000 -s /bin/bash -m simple-user
COPY --chown=simple-user:simple-user entrypoint.sh /opt/entrypoint.sh
RUN chmod +x /opt/entrypoint.sh && \
    DEBIAN_FRONTEND=noninteractive apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get upgrade -y && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends tzdata curl gnupg gnupg2 gnupg1 apt-transport-https ca-certificates iputils-ping snmp procps lm-sensors && \
    curl -sL https://repos.influxdata.com/influxdb.key | apt-key add - && \
    export $(head -n 3 /etc/lsb-release) && \
    echo "deb https://repos.influxdata.com/ubuntu ${DISTRIB_CODENAME} stable" | tee /etc/apt/sources.list.d/influxdb.list && \
    DEBIAN_FRONTEND=noninteractive apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install telegraf && \
    DEBIAN_FRONTEND=noninteractive apt-get purge -y curl gnupg gnupg2 gnupg1 apt-transport-https && \
    DEBIAN_FRONTEND=noninteractive apt-get -y autoremove && \
    DEBIAN_FRONTEND=noninteractive apt-get clean && \
    rm -rf /var/lib/apt/lists/*
USER 10000
WORKDIR /home/simple-user
ENTRYPOINT ["/opt/entrypoint.sh"]
CMD ["telegraf"]

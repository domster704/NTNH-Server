# NTNH Server Docker Image
FROM eclipse-temurin:8-jre

LABEL maintainer="NTNH Team"
LABEL description="NTNH Modpack Server for Minecraft 1.7.10"

RUN apt-get update && apt-get install -y bash && rm -rf /var/lib/apt/lists/*

WORKDIR /server

# Copy local repository contents into the image
COPY . /server/

RUN mkdir -p /server/world /server/backups /server/logs /server/crash-reports

EXPOSE 25565/tcp
EXPOSE 25565/udp

VOLUME ["/server/world", "/server/backups", "/server/logs", "/server/crash-reports"]

ENV JVM_OPTS="-Xms4G -Xmx8G -XX:+UseG1GC -XX:+UnlockExperimentalVMOptions -XX:MaxGCPauseMillis=100"
ENV EULA="false"
ENV SERVER_PORT="25565"

COPY --chmod=755 <<'EOF' /server/docker-entrypoint.sh
#!/bin/bash
set -e

echo "Starting NTNH Server..."

if [ "$EULA" != "true" ]; then
    echo "ERROR: You must accept the EULA. Set EULA=true environment variable."
    exit 1
fi

echo "eula=true" > eula.txt

exec java $JVM_OPTS -jar forge-1.7.10-10.13.4.1614-1.7.10-universal.jar nogui
EOF

ENTRYPOINT ["/server/docker-entrypoint.sh"]

FROM registry.access.redhat.com/ubi8/openjdk-8:latest as builder

USER 0

WORKDIR /tmp/app

COPY src/ src/
COPY pom.xml pom.xml

RUN mvn clean install

FROM docker.io/library/open-liberty:21.0.0.12-full-java8-openj9

ARG VERSION=1.0
ARG REVISION=SNAPSHOT

LABEL \
  org.opencontainers.image.authors="Your Name" \
  org.opencontainers.image.vendor="IBM" \
  org.opencontainers.image.url="local" \
  org.opencontainers.image.source="https://github.com/OpenLiberty/guide-getting-started" \
  org.opencontainers.image.version="$VERSION" \
  org.opencontainers.image.revision="$REVISION" \
  vendor="Open Liberty" \
  name="system" \
  version="$VERSION-$REVISION" \
  summary="The system microservice from the Getting Started guide" \
  description="This image contains the system microservice running with the Open Liberty runtime."

USER 0

WORKDIR /config

RUN mkdir /tmp/certs && \
  cd /tmp/certs && \
  openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out /tmp/certs/ca.crt -subj "/C=FR/O=example/OU=Domain Control Validated/CN=*.example.io" && \
  keytool -keystore $JAVA_HOME/lib/security/cacerts -storepass changeit -noprompt -trustcacerts -importcert -alias whatever -file /tmp/certs/ca.crt

COPY --chown=1001:0 src/main/liberty/config/ /config/
COPY --chown=1001:0 --from=builder /tmp/app/target/*.war /config/apps/

USER 1001

RUN configure.sh

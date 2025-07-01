ARG VERSION=0.6.1

FROM debian:stable as downloader

ARG VERSION

RUN apt-get update && apt-get install -y curl
RUN mkdir -p /app/x86 /app/arm64 /app/armv7 /app/extracted/x86 /app/extracted/arm64 /app/extracted/armv7


COPY smartdns-x86_64-unknown-linux-musl  /app/x86/smartdns.tar.gz
COPY smartdns-aarch64-unknown-linux-musl   /app/extracted/arm64
COPY smartdns-aarch64-unknown-linux-musl /app/armv7/smartdns.tar.gz

FROM alpine as base

WORKDIR /app
RUN apk add bash
COPY docker-entrypoint.sh /app/entrypoint.sh
FROM base as amd64

COPY --from=downloader /app/extracted/x86/* /app/

FROM base as armv7

COPY --from=downloader /app/extracted/armv7/* /app/

FROM base as arm64

COPY --from=downloader /app/extracted/arm64/* /app/


FROM ${TARGETARCH}${TARGETVARIANT} as final


EXPOSE 8000
CMD ["bash", "/app/entrypoint.sh"]

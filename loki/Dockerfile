ARG VERSION=3.4

FROM grafana/loki:${VERSION}

# Run as root so the mounted Railway volume at /loki is writable
USER root

ENV LOKI_RETENTION_PERIOD=744h
ENV LOKI_RETENTION_ENABLED=true

COPY loki.yml /etc/loki/loki-config.yaml

CMD ["-config.file=/etc/loki/loki-config.yaml", "-config.expand-env=true"]

ARG VERSION=2.9.0

FROM grafana/tempo:${VERSION}

# Run as root so the mounted Railway volume at /var/tempo is writable
USER root

COPY tempo.yml /etc/tempo/tempo.yml

CMD ["-config.file=/etc/tempo/tempo.yml"]
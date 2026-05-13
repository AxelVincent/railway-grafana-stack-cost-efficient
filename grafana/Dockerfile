ARG VERSION=11.5.2

FROM grafana/grafana-oss:${VERSION}

# Run as root so the mounted Railway volume at /var/lib/grafana is writable
USER root

# copy the datasources and dashboards
COPY datasources /etc/grafana/provisioning/datasources
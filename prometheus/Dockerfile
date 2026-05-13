ARG VERSION=v3.2.1

FROM prom/prometheus:${VERSION}

# Run as root so the mounted Railway volume at /prometheus is writable
USER root

# Copy the prom.yml configuration file to the container
COPY prom.yml /etc/prometheus/prom.yml

# Command to run Prometheus
CMD ["--config.file=/etc/prometheus/prom.yml", "--storage.tsdb.path=/prometheus"]
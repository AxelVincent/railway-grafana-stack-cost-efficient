# Deploy and Host Grafana Stack Cost Efficient on Railway

Grafana Stack Cost Efficient is a one-click observability stack bundling Grafana, Loki, Prometheus, and Tempo. The services come pre-wired so dashboards, metrics, logs, and traces work out of the box, with configurable log retention via Loki's compactor to keep storage costs predictable on long-running deployments.

## About Hosting Grafana Stack Cost Efficient

Hosting this stack on Railway provisions four interconnected services from a single repository: Grafana for visualization, Loki for log aggregation, Prometheus for metrics, and Tempo for distributed traces. Each service runs from its own Docker image with a pinnable `VERSION` variable, a dedicated persistent volume for data durability across redeploys, and a private domain for internal communication. Only Grafana is exposed publicly through Railway's HTTP proxy; the three backends stay on the private network. Cross-service references resolve automatically so Grafana's datasources connect without manual URL editing, and the Loki retention compactor enforces a configurable cleanup window so your log storage doesn't grow unbounded.

## Common Use Cases

- Centralized observability for Railway-hosted applications — collect logs via Locomotive, scrape metrics from your services, and ingest OpenTelemetry traces into one Grafana instance
- Cost-controlled long-term log storage with a tunable retention period instead of paying for indefinite log storage on managed observability vendors
- Self-hosted alternative to Grafana Cloud, Datadog, or New Relic for teams that want full data ownership and predictable monthly spend

## Dependencies for Grafana Stack Cost Efficient Hosting

- A Railway account with billing enabled (the stack runs four services plus four persistent volumes)
- A GitHub account if you intend to fork the repository to customize Loki, Prometheus, or Tempo configuration files

### Deployment Dependencies

- [Grafana documentation](https://grafana.com/docs/grafana/latest/)
- [Loki documentation](https://grafana.com/docs/loki/latest/)
- [Prometheus documentation](https://prometheus.io/docs/introduction/overview/)
- [Tempo documentation](https://grafana.com/docs/tempo/latest/)
- [Locomotive — Railway log shipper for Loki](https://railway.com/template/jP9r-f)

### Implementation Details

After deployment, configure your other Railway services to send telemetry into the stack by referencing the internal URLs exposed on the Grafana service:

```
LOKI_URL=${{Grafana.LOKI_INTERNAL_URL}}
PROMETHEUS_URL=${{Grafana.PROMETHEUS_INTERNAL_URL}}
TEMPO_URL=${{Grafana.TEMPO_INTERNAL_URL}}
```

To send OpenTelemetry traces to Tempo, use the dedicated ingest variables on the Tempo service (HTTP endpoint accepts `/v1/traces`):

```
TEMPO_HTTP=${{Tempo.INTERNAL_HTTP_INGEST}}
TEMPO_GRPC=${{Tempo.INTERNAL_GRPC_INGEST}}
```

To change log retention without forking, set `LOKI_RETENTION_PERIOD` on the Loki service to any multiple of `24h` (e.g. `744h` for 31 days, `2160h` for 90 days). To keep logs indefinitely, set `LOKI_RETENTION_ENABLED=false`. Both variables are expanded into Loki's config at startup, so changes take effect on the next redeploy of the Loki service.

## Why Deploy Grafana Stack Cost Efficient on Railway?

Railway is a singular platform to deploy your infrastructure stack. Railway will host your infrastructure so you don't have to deal with configuration, while allowing you to vertically and horizontally scale it.

By deploying Grafana Stack Cost Efficient on Railway, you are one step closer to supporting a complete full-stack application with minimal burden. Host your servers, databases, AI agents, and more on Railway.

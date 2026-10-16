# Enterprise Microservices Radar

- **as_of**: 2026-10-16
- **owner**: Architecture Guild
- **environment**: Production

## Services

| Name | Service Mesh | Runtime | Database | P99 Latency (ms) | Uptime (%) | Tier |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| auth-gateway | Istio | Go 1.22 | Redis 7.2 | 14 | 99.99 | Critical |
| billing-ledger | Linkerd | PHP 8.3 Laravel | PostgreSQL 16 | 38 | 99.95 | Critical |
| notification-hub | Consul | Node.js 20 | MongoDB 7.0 | 85 | 99.90 | Standard |
| order-router | Istio | Java 21 Spring | MySQL 8.4 | 42 | 99.99 | Critical |
| identity-vault | Linkerd | Rust | PostgreSQL 16 | 8 | 99.99 | Critical |
| recommendation-engine | Kuma | Python 3.12 | Qdrant | 120 | 99.80 | Analytics |
| search-indexer | Kuma | Go 1.22 | Elasticsearch 8 | 65 | 99.90 | Standard |
| payment-webhook-gateway | Istio | PHP 8.3 Laravel | Redis 7.2 | 22 | 99.99 | Critical |
| reporting-batch | None | Python 3.12 | ClickHouse | 450 | 99.50 | Batch |
| audit-log-pipeline | Consul | Go 1.22 | ClickHouse | 35 | 99.90 | Standard |
| inventory-sync | Linkerd | Go 1.22 | MySQL 8.4 | 48 | 99.95 | Critical |
| cart-session-store | Istio | Rust | Redis 7.2 | 6 | 99.99 | Critical |
| catalog-read-api | Istio | Go 1.22 | MongoDB 7.0 | 18 | 99.95 | Standard |
| shipping-calculator | Consul | Python 3.12 | PostgreSQL 16 | 92 | 99.85 | Standard |
| discount-rule-engine | Linkerd | PHP 8.3 Laravel | Redis 7.2 | 19 | 99.90 | Standard |
| fraud-detection-stream | Istio | Python 3.12 | Kafka+Redis | 45 | 99.95 | Critical |
| analytics-event-collector | Kuma | Go 1.22 | ClickHouse | 12 | 99.90 | Standard |
| cdn-edge-origin | None | Nginx Lua | Redis 7.2 | 4 | 99.99 | Critical |
| customer-crm-sync | Consul | Node.js 20 | PostgreSQL 16 | 110 | 99.70 | Batch |
| api-token-authorizer | Istio | Rust | Redis 7.2 | 5 | 99.99 | Critical |

## Operational Rules & Verification Constraints

- P99 latency above 100ms triggers automatic scaling alert.
- Critical tier services require multi-region Redis failover and double-entry reconciliation.
- Zero unencrypted inter-service RPC allowed across all service meshes.

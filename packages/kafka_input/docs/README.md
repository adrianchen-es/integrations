# Custom Kafka Input package

This is a **standalone input** package for Elastic Agent (same packaging style as the [Custom WMI Input](https://github.com/elastic/integrations/tree/main/packages/wmi) package). It exposes the Filebeat **Kafka** input in Fleet so you can consume one or more topics and choose a dataset name for routing.

## Requirements

Elastic Agent with a stack version that satisfies the package manifest conditions (see `manifest.yml`).

## Configuration

Configure bootstrap **hosts**, **topics**, a consumer **group_id**, and the **dataset** name. Optional settings cover SASL, Kerberos, SSL, parsers, processors, and advanced fetch / rebalance tuning — mirroring the Kafka stream options used in the `kafka_log` integration’s generic data stream.

See the Filebeat documentation for the [Kafka input](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-input-kafka.html) for behavior details.

## Tests (compare with `kafka_log`)

- **System tests** live under `_dev/test/system/` with package-level `_dev/deploy/docker/docker-compose.yml`, matching other **input** packages (`filestream`, `prometheus_input`): top-level `vars` (not `data_stream.vars`), `service` set to the Kafka broker service, and `input: kafka`.
- **`kafka_log`** adds **pipeline tests** only on the **metrics** data stream because that stream ships `elasticsearch/ingest_pipeline/default.yml`. **`kafka_input` has no ingest pipeline**, so `elastic-package test pipeline` does not apply unless you add pipelines and field definitions later.
- **“Metrics” over Kafka here** means the broker delivers **JSON lines** (for example `{"metric":{"name":...,"value":...}}`); the agent stores them like any other Kafka message, typically with the payload in `message`. Turning that into ECS metric documents (for example `event.kind: metric` and extracted fields) is what **`kafka_log`’s metrics data stream + default pipeline** does—either use that integration, attach a **custom ingest pipeline** in Fleet for this input, or extend `kafka_input` with its own pipeline and `_dev/test/pipeline/` fixtures.

Run from the package directory:

```bash
elastic-package test system
```

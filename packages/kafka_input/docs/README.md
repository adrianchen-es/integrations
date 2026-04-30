# Custom Kafka Input package

This is a **standalone input** package for Elastic Agent (same packaging style as the [Custom WMI Input](https://github.com/elastic/integrations/tree/main/packages/wmi) package). It exposes the Filebeat **Kafka** input in Fleet so you can consume one or more topics and choose a dataset name for routing.

## Requirements

Elastic Agent with a stack version that satisfies the package manifest conditions (see `manifest.yml`).

## Configuration

Configure bootstrap **hosts**, **topics**, a consumer **group_id**, and the **dataset** name. Optional settings cover SASL, Kerberos, SSL, parsers, processors, and advanced fetch / rebalance tuning — mirroring the Kafka stream options used in the `kafka_log` integration’s generic data stream.

See the Filebeat documentation for the [Kafka input](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-input-kafka.html) for behavior details.



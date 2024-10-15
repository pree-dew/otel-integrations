## Otel Integrations

This repo is created to provide setups to do quick solutioning for shippping data(metrics/logs/traces) from different input sources to otlp compatible backend or otel collector itself.
Understand pros and cons of different solutions and improvise according to production requirements.

### Use Case 1: Ship logs from s3 to otel
* [Push via Logstash using http](https://github.com/pree-dew/otel-integrations/tree/main/from-s3-to-otel-collector/via-logstash/s3-to-otel-collector-via-http)
* [Push via Logstash using tcp](https://github.com/pree-dew/otel-integrations/tree/main/from-s3-to-otel-collector/via-logstash/s3-to-otel-collector-via-tcp)

# Integration Tests

## Dynamic configuration of proxy

slide #2
Grey Matter enables you to dynamically configure our proxy.

## Three Body Problem

slide #3

* proxies subscribe to gm-control xds feed
* gm-control polls gm-control-api once per second for changes to proxies

Send a JSON object to gm-control-api.
gm-control picks up the change and enables gm-proxy/Envoy to discover the
change through one of the xDS protocols.

Walk through the 3 body problem two times

* one from the terminal
* one in the actual integration tests

### from the terminal

* run docs/examples/integration
  
```
source ./env.sh
```

```
greymatter edit listener listener-example
```

* set `active_http_filters` (notice the '.')
  
```json
"active_http_filters": ["gm.metrics"],
```

* set `http_filters{"gm_metrics"}` (notice the '_')

```json
"http_filters": {
    "gm_metrics": {
        "metricsPort":                              8081,
        "metricsHost":                             "0.0.0.0",
        "metricsDashboardUriPath":                 "/metrics",
        "metricsPrometheusUriPath":                "/prometheus",
        "prometheusSystemMetricsIntervalSeconds":  15,
        "metricsRingBufferSize":                   4096,
        "metricsKeyFunction":                     "none"
    }
},
```

```
curl localhost:8001/config_dump | view -
```

```
/http_filters
```

### from the tests

* TestMain definitions
  * compared to docker-compose.yaml 


* func (state *TestState) testSetListenerMetricsFilter() error {
  

* traversal <https://github.com/dougfort/traversal>
WARNING: THIS REPO HAS beEN MOVED TO https://github.com/solo-io/isotope

# Isotope and Converter

Inspired by [Istio's Isotope](https://github.com/istio/tools/tree/master/isotope)

## Run

### Convert

Develop
```
cd convert

go run main.go kubernetes ../samples/loadgenerator.yaml --service-image antonioberben/isotope-service --client-image fortio/fortio --client-namespace default --cluster cluster1 > ../cluster1.yaml

go run main.go kubernetes ../samples/loadgenerator.yaml --service-image antonioberben/isotope-service --client-image fortio/fortio --client-namespace default --cluster cluster1 > ../cluster2.yaml
```

or
```
go run main.go kubernetes ../samples/loadgenerator.yaml --service-image antonioberben/isotope-service --cluster cluster1  > ../cluster1.yaml

go run main.go kubernetes ../samples/loadgenerator.yaml --service-image antonioberben/isotope-service --cluster cluster2  > ../cluster2.yaml
```

Build:
```
export KO_DOCKER_REPO=antonioberben/isotope-convert
ko build --bare -t 0.9.1-alpha3 .
```

Run:
```
docker run -it \
    -v $(pwd)/samples/loadgenerator.yaml:/etc/config/service-graph.yaml  \
    antonioberben/isotope-convert:0.9.1-alpha3  kubernetes /etc/config/service-graph.yaml  --service-image antonioberben/isotope-service:0.9.1-alpha3  --client-image fortio/fortio > manifests.yaml
```

### Isotope

Develop:
```
cd service
SERVICE_NAME=client CONFIG_PATH=service-graph.yaml go run main.go
```

Build:
```
export KO_DOCKER_REPO=antonioberben/isotope-service
ko build --bare -t 0.9.1-alpha4 .
```




# Service

This directory holds the "mock-service" component for isotope. It is a
relatively simple HTTP server which follows instructions from a YAML file and
exposes Prometheus metrics.

## Usage

1. Include the entire topology YAML in `/etc/config/service-graph.yaml`
1. Set the environment variable, `SERVICE_NAME`, to the name of the service
   from the topology YAML that this service should emulate

## Metrics

Captures the following metrics for a Prometheus endpoint:

- `service_incoming_requests_total` - a counter of requests received by this
  service
- `service_outgoing_requests_total` - a counter of requests sent to other
  services
- `service_outgoing_request_size` - a histogram of sizes of requests sent to
  other services
- `service_request_duration_seconds` - a histogram of durations from "request
  received" to "response sent"
- `service_response_size` - a histogram of sizes of responses sent from this
  service

## Performance

With both a Fortio 1.1.0 client and a single isotope service running in a GKE
cluster, the client on a n1-highcpu-96 machine sending 96 concurrent requests
as fast as possible, the service on a n1-standard-4 machine with a limit of 1
vCPU and 3.75GB RAM, and logging set to INFO, the service reaches a maximum
QPS of 12,000 - 14,000 before maxing out CPU.

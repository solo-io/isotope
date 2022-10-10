# Isotope and Converter

Inspired by [Istio's Isotope](https://github.com/istio/tools/tree/master/isotope), and from which most of the code has been taken,
this tool helps on two major tasks:

- Deploy a set of mock services through a human readable graph-scheme yaml to simulate a entire system/s with service-to-service communications
- Design Load Testing scenarios for mutliple cluster environments in [Gloo Mesh](https://docs.solo.io/gloo-mesh-enterprise/latest/)

In example:

A user can deploy multiple mock services defining a chain of communication:
```
ServiceA (in cluster 1) calls:
  ServiceB (in cluster1)
  ServiceC (in cluster2)
ServiceC calls:
  ServiceD (in cluster2)
```

Then, the user can create the Gloo Mesh resources (such as VirtualDestinations, VirtualGateways, RouteTables) using those mock service without th need of the real services.

Finally, the graph-scheme offers the possibility to inject errors, heavy payloads, sleeps, etc. to deploy those services.
So that operators can create Gloo Mesh resources (such as FailoverPolicy) before hand, which allows them to analyse the future
of the mesh.

Finally, one of the deployed service is [fortio](https://github.com/fortio/fortio), the load testing tool used in Istio.
This tools can be used as a standard simple testing tool for Gloo Mesh users to use.


## Acknowledgements
Isotope is built upon other open source code projects. Without these projects Isotope would never have seen the light.

- [Istio's Isotope](https://github.com/istio/tools/tree/master/isotope)
- [Fortio](https://github.com/fortio/fortio)
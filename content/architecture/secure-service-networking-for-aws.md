---
title: 'Secure Service Networking for AWS'
descriptions: 'Reference Architecture for implementing mulit-platform Secure Service Networking on AWS'
date: Monday, 10 Oct 2022
draft: false
tags: ['Reference Architecture', 'Zero Trust', 'Security', 'AWS']
featured_image: ''
omit_header_text: true
toc: true
menu:
  main:
    parent: architecture
---

## Design Objectives

Create a Reference Architecture for Secure Service Networking best practices on the AWS Cloud Platform.

### Requirements

Apply the [Principles of Zero Trust]({{< ref "/articles/072-princples-of-zero-trust.md" >}} "Principles of Zero Trust") security across multiple AWS platforms, e.g. EKS, ECS, and EC2.

### Exclusions

The underlying virtual infrastructure terraform deployment has not been hardened for production use. The focus is on the Secure Service Networking applied to said infrastructure.

---

## Design Considerations

### All Releases

#### Terraform Modules

Consul is tested against AWS VPC Module 17.

### RELEASE v0.3.0 - Replace HCP Consul w/ 'Self-managed' Consul to support Vault Secrets managements

The primary objective was to add 'Access Security' (aka Data at Rest security) to the AWS platforms and Secure Service Networking platform. This requires the replacement of HCP Consul with 'self-managed' (Manual Service install) Consul Enterprise.

The version 0.3.0 release goals included:

* Vault Secrest Management
* Vault for Secure Service Network Certificate Authority
* Vault for Gossip Key Rotation

The objective was to add secure secrets management (HashiCorp Vault) enabling the application of Zero Trust Principles to both data in motion (service mesh/secure service networking) and data at rest (secrets management).

### RELEASE v0.2.0 - Add Observability with Jaeger, Prometheus, and Grafana

The primary objective was to add observability to secure service networking.

The version 0.2.0 release goals included:

Add Observability:

* Enable Consul tracing in helm chart
* Install Jaeger
* Install Prometheus
* Install Grafana

Add more Troubleshooting:

* Add more kubectl remote exec examples
* Add envoy container troubleshooting steps

### RELEASE v0.1.1 - Pin all the terraform versions

* PIN all the versions for consistent good behavior.

### RELEASE v0.1.0 - Secure Service Netorking across EKS and ECS platforms

The objective was to establish secure communications across multiple platforms and apply the principes of zero trust security to data in motion (machine to machine access). This release did not include secure secrets management and the application of zreo trust principles to data at rest (see v0.3.0). 

The version 0.1.0 release goals included:

* 1x HCP Consul cluster running Consul v1.11.x
* 3x VPCs
* 3x Consul Admin Partitions serving:
  * 2x EKS clusters
  * 1x ECS clusters

---

## Known issues

### v0.1.0 - Secure Service Netorking across EKS and ECS platforms

### v0.2.0 - Add Observability with Jaeger, Prometheus, and Grafana

### v0.3.0 - Replace HCP Consul w/ 'Self-managed Consul to support Vault Secrets managements


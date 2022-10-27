---
title: 'Secure Service Networking on AWS for Dev/Test'
date: '2022-10-10'
draft: false
descriptions: 'Reference Architecture for implementing mulit-platform Secure Service Networking on AWS for Dev/Test'
tags: ['Reference Architecture', 'Zero Trust', 'Security', 'AWS']
featured_image: ''
omit_header_text: true
toc: true
---

## Design Objective

Create a Reference Architecture for Secure Service Networking best practices on the AWS Cloud Platform for Dev/Test.

### Requirements

Apply the [Principles of Zero Trust]({{< ref "/articles/072-princples-of-zero-trust.md" >}} "Principles of Zero Trust") security across multiple AWS platforms, e.g. EKS, ECS, and EC2.

### Exclusions

The underlying virtual infrastructure deployed by terraform has not been hardened for production use. The focus is on the Secure Service Networking applied to said infrastructure.

---

## Ref Arch Structure 

Layout of this Reference Architecture:

1. Microservice application
2. Secure Service Networking
3. Infrastructure
4. Troubleshooting

### 1. Microservice Application

Developed for demonstration purposes, the REDtalks.live Chat App employs a microservice architecture comprised of a:
* web frontend utilizing the Feathers.js web framework
* middleware layer for chat/bot automation
* MongoDB for user credentials and chat history

#### Design Considerations

Demonstrate multiple container to container, container to virtual machine, and container to external SaaS/API based resource communication paths.

### 2. Secure Service Networking

Apply the [Principles of Zero Trust]({{< ref "/articles/072-princples-of-zero-trust.md" >}} "Principles of Zero Trust") to the micro-serivce Chat App ensuring said principles are applied not only towards containter to container commuications but also for, container to VM, and container to SaaS/API based resources.

Primary focus:
* Principles of ZT
* Service Mesh
* Secrets Managmenet
* Mesh Certificate Management
* Observability

#### Design Considerations

* Multi-platform
* Secrets Management - Secure Data at Rest
* Secure Service Networking - Secure Data in Motion

##### Multi-platform

The term "Service Mesh" is often related only to k8s implementation. However, the benefits of Secure Service Networking is equally important to all platforms, including virtual machines and non-k8s container management soltuions.

##### Secrets Management

Like k8s, many systems don't encrypt platform/system secrets (keys, IDs, tokens, etc). By integrating HashiCorp Vault with the Service Mesh, both the transport and the secrets to deliver it, can be highly secure.

##### Secure Service Networking

As with the importance of encrypting platform/system secrets, Secure Service Networking ensure the encryption of data in transit, for both k8s and other platforms.

### 3. Infrastructure

1. VPC
2. EC2
3. EKS


#### Design Considerations

The purpose of the underlying infrastructure is to be stable. No effort has been made to create a hardened, production kubernetes platform at this stage. Consequently, for the most part, defaults were
accepted and the provider/module versions mimiced that of the consul-k8s acceptance tests.
#### Terraform Provider/Module Versions

Per the Consul-k8s acceptance tests, the following provider/module versions are used:
https://github.com/hashicorp/consul-k8s/tree/main/charts/consul/test/terraform/eks

*AWS Provider >= 2.28.1*

https://github.com/hashicorp/consul-k8s/blob/main/charts/consul/test/terraform/eks/main.tf#L2

*AWS VPC Module v3.11.0*

https://github.com/hashicorp/consul-k8s/blob/main/charts/consul/test/terraform/eks/main.tf#L28

*AWS EKS Module v17.24.0*

https://github.com/hashicorp/consul-k8s/blob/main/charts/consul/test/terraform/eks/main.tf#L57

*EKS Cluster v1.21*

https://github.com/hashicorp/consul-k8s/blob/main/charts/consul/test/terraform/eks/main.tf#L61


In addition to the provider/modules utilized in the acceptance test, the following were also used:


### 4. Troubleshooting

Link to:
* HT-Troubleshoot Consul Helm Chart Install
* HT-Troubleshoot Consul Bootstrap
* HT-Troubleshoot EKS Security Groups

---


### RELEASE v0.3.0 - Replace HCP Consul w/ 'Self-managed' Consul to support Vault Secrets managements

The primary objective was to add 'Access Security' (aka Data at Rest security) to the AWS platforms and Secure Service Networking platform. This requires the replacement of HCP Consul with 'self-managed' (Manual Service install) Consul Enterprise.

The version 0.3.0 release goals included:

* Vault Secrest Management
* Vault for Secure Service Network Certificate Authority
* Vault for Gossip Key Rotation

The objective was to add secure secrets management (HashiCorp Vault) enabling the application of Zero Trust Principles to both data in motion (service mesh/secure service networking) and data at rest (secrets management).

---

## Previous Releases

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


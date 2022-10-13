---
title: '#070 - Consul Terraform Sync - Under the Hood!'
date: '2021-02-02'
draft: false
tags: ['Consul', 'Terraform', 'NIA', 'Consul Terraform Sync']
---

Following last weeks episode – [#69 – Network Infrastructure Automation w/ Consul](https://redtalks.live/2021/01/25/69-network-infrastructure-automation-w-consul/) – we're going to take a closer look at HashiCorp's _Consul Terraform Sync_, and how it stitches together the observability of Consul with the unmatched Infrastructure as Code capabilities of Terraform.

![](https://redtalkslive.files.wordpress.com/2021/01/nia-cts-workflow.png?w=1024)

Network Infrastructure as Code workflow

In this episode you'll get a deeper look at what an NIA module is and how that consumes Consul's all-seeing, all-knowing application state and prepares it for network infrastructure configuration.

**_Short version:_** Industry proven enterprise technologies (Consul and Terraform) stitched together by a simple, elegant state transfer module.

{{< youtube q9HXUDiep0Y >}}

Example state emitted from Consul:

```
services = {
  "web.web-vm-000000.default.east-us" : {
    id      = "web"
    name    = "web"
    address = "10.3.4.9"
    port    = 9090
    meta = {
      AS3TMPL = "http"
      VSIP    = "10.3.3.4"
      VSPORT  = "80"
    }
    tags            = []
    namespace       = "default"
    status          = "passing"
    node            = "web-vm-000000"
    node_id         = "6c908fe0-779e-cabe-ebeb-e77815c2b8f4"
    node_address    = "10.3.4.9"
    node_datacenter = "east-us"
    node_tagged_addresses = {
      lan      = "10.3.4.9"
      lan_ipv4 = "10.3.4.9"
      wan      = "10.3.4.9"
      wan_ipv4 = "10.3.4.9"
    }
    node_meta = {
      consul-network-segment = ""
    }
  },
  "web.web-vm-000001.default.east-us" : {
    id      = "web"
    name    = "web"
    address = "10.3.4.10"
    port    = 9090
    meta = {
      AS3TMPL = "http"
      VSIP    = "10.3.3.4"
      VSPORT  = "80"
    }
    tags            = []
    namespace       = "default"
    status          = "passing"
    node            = "web-vm-000001"
    node_id         = "15e85764-70f4-d563-edc2-cd66113a6982"
    node_address    = "10.3.4.10"
    node_datacenter = "east-us"
    node_tagged_addresses = {
      lan      = "10.3.4.10"
      lan_ipv4 = "10.3.4.10"
      wan      = "10.3.4.10"
      wan_ipv4 = "10.3.4.10"
    }
    node_meta = {
      consul-network-segment = ""
    }
  }
}
```

Example Consul Terraform Sync configuration file:

```
# Global Config Options
log_level = "info"
buffer_period {
  min = "5s"
  max = "20s"
}

# Consul Config Options
consul {
  address = "localhost:8500"
  token = "<mega_awesome_consul_token>"
}

# Terraform Driver Options
driver "terraform" {
  log = true
  path = "/opt/consul-tf-sync.d/"
  working_dir = "/opt/consul-tf-sync.d/"
  required_providers {
    bigip = {
      source = "F5Networks/bigip"
    },
    panos = {
      source = "PaloAltoNetworks/panos"
    }
  }
}

## Network Infrastructure Options

# BIG-IP Workflow Options
terraform_provider "bigip" {
  address = "1.1.1.2:8443"
  username = "f5admin"
  password = "<password>"
}

# Palo Alto Workflow Options
terraform_provider "panos" {
  alias = "panos1"
  hostname = "1.1.1.3"
#  api_key  = "<api_key>"
  username = "paloalto"
  password = "<password>"
}

## Consul Terraform Sync Task Definitions

# Load-balancer operations task
task {
  name = "F5-BIG-IP-Load-Balanced-Web-Service"
  description = "Automate F5 BIG-IP Pool Member Ops for Web Service"
  source = "f5devcentral/app-consul-sync-nia/bigip"
  providers = ["bigip"]
  services = ["web"]
}

# Firewall operations task
task {
  name = "DAG_Web_App"
  description = "Automate population of dynamic address group"
  source = "PaloAltoNetworks/ag-dag-nia/panos"
  providers = ["panos.panos1"]
  services = ["web"]
  variable_files = ["/etc/consul-tf-sync.d/panos.tfvars"]
}
```
---
title: "HT: Retrieve K8s Service Account names"
date: Fri, 21 Oct 2022
draft: false
description: "Understand how to find the K8s Service Account names for Consul"
#featured_image: "/images/Pope-Edouard-de-Beaumont-1844.jpg"
tags: ["kb", "howto", "helm", "k8s"]
featured_image: ''
omit_header_text: true
---

Documented at: 10/07/2022
Review by: 12/01/2023

The following command, referenced freqently in Consul documetation doesn't work with unique `global.name`:

`helm template --release-name consul -s templates/client-daemonset.yaml hashicorp/consul --version=0.47.1 | yq r - metadata.name`

The output from the above command is `consul-consul-client`. However, When using Consul Admin Partitions, it is require to use uniqe `global.name` entries per helm install/k8s cluster. To ensure that the above command considers the unqique `global.name` operators must also add their helm values to the command, as such:

`helm template --release-name consul -s templates/client-daemonset.yaml -f ./consul_chart_values.yaml hashicorp/consul --version=0.47.1 | yq r - metadata.name`

Assuming the above `./consul_chart_values.yaml` file includes `global.name = team1`, the correct Service Account name would now be output:

`consul-team1-client`

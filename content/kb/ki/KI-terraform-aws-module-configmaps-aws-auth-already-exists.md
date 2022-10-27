---
title: 'KNOWN ISSUE: Error: configmaps "aws-auth" already exists'
date: '2022-10-14'
draft: false
description: "Workaround for AWS EKS module v17.x timing issue whereby the 'aws-auth' configmap exists before it was expected to"
tags: ["Known Issue", "AWS", "terraform", "module", "k8s"]
featured_image: ''
omit_header_text: true
---

Documented at: 10/14/2022
Review by: 12/01/2023

## Overview

The following is a common error with AWS EKS Module v17.x:

`Error: configmaps "aws-auth" already exists`

Example module use:

```hcl
module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "17.20.0"

  ...

}
```

Full error example:

```hcl
│ Error: configmaps "aws-auth" already exists
│ 
│   with module.eks.kubernetes_config_map.aws_auth[0],
│   on .terraform/modules/eks/aws_auth.tf line 63, in resource "kubernetes_config_map" "aws_auth":
│   63: resource "kubernetes_config_map" "aws_auth" {
```

## Workaround:

Delete the `aws-auth` config map using `kubectl` and re-run the Terraform apply:

```sh
kubectl -n kube-system delete cm aws-auth
terraform apply
```
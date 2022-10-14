---
title: 'KNOWN ISSUE: terraform destroy fails on EKS cluster due to public subnet'
date: '2022-10-14'
draft: false
description: "Workaround for AWS EKS module v17.x failing to destroy due to public subnet destroy timeout"
tags: ["Known Issue", "AWS", "terraform", "module", "k8s"]
featured_image: ''
omit_header_text: true
---

Documented at: 10/14/2022
Review by: 12/01/2023

## Overview

Reaching an eventual timeout with the following is a common error with AWS EKS Module v17.x:

```sh
module.vpc.aws_subnet.public[0]: Still destroying...
```

When exposing an EKS service externally this can result in the creating of an external/public IP (AWS EIP). Terraform doesn't know about this IP Address and hence it is not listed in the terraform state file as an object/resource for deletion. AWS doesn't automatically delete EIPs that are bound to the EKS node interface. 


## Workaround:

Option 1) uninstall the service that created the EIP before running the `terraform destroy`. This could be the Consul Ingress Gateway, or any externally accessible service.

Option 2) if you've already run the terraform destroy and it has timed out, using the aws cli, or AWS console, you will need to find and manually delete the EIPs before re-running the terraform destroy to cleanup the remaining resources.

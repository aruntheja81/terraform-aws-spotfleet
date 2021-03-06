## Terraform spotfleet module

[![Build Status](https://travis-ci.com/telia-oss/terraform-aws-spotfleet.svg?branch=master)](https://travis-ci.com/telia-oss/terraform-aws-spotfleet)

Easy way of setting up a spot request, which also takes care of creating:

- Security group (including public egress).
- IAM role with instance profile.

This module comes with 6 predefined spot requests

 |#| Name | Description|
 |--- |--- |---|
 |1. | small |request includes single vCPU instances and includes older generation instances|
 |2. | small-ipv6 | request includes single vCPU instances but excludes all instances that do not support IPV6|
 |3. | medium | request includes instances with a minimum of 2 vCPUs and includes older generation instances|
 |4. | medium-ipv6 | request includes instances with a minimum of 2 vCPUs but excludes all instances that do not support IPV6|
 |5. | large | request includes instances with a minimum of 4 vCPUs and includes older generation instances|
 |6. | large-ipv6 | request includes instances with a minimum of 4 vCPUs but excludes all instances that do not support IPV6|

#### Gotchas

##### Need to manually delete instances created
If your Spot request is active and has an associated running Spot Instance, "Canceling the request does not terminate
the instance; you must terminate the running Spot Instance manually."  This means that terraform destroy will not
get rid of everything you have created.  In the case of a script that creates a VPC and a spotfleet cluster you will
need to manually delete the EC2 instances created by the spot before terraform destroy can complete.

##### End date
Spot requests have an end date.  After this date instances that are terminated (manually or due to price point being
exceeded) are not replaced.

Note: By default spot fleets created by this module will become inactive on 2028-05-03T00:00:00Z

##### 3 Subnets
The pre-defined spot request compositions are hard coded to use 3 subnets
# Web server cluster example (production environment)

This folder contains example [Terraform](https://www.terraform.io/) templates that deploy a cluster of web servers 
(using [EC2](https://aws.amazon.com/ec2/) and [Auto Scaling](https://aws.amazon.com/autoscaling/)) and a load balancer
(using [ELB](https://aws.amazon.com/elasticloadbalancing/)) in an [Amazon Web Services (AWS) 
account](http://aws.amazon.com/). The load balancer listens on port 80 and returns the text "Hello, World" for the 
`/` URL. The Auto Scaling Group is able to do a zero-downtime deployment when you update any of it's properties. The 
code for the cluster and load balancer are defined as a Terraform module in
[modules/services/webserver-cluster](../../../../modules/services/webserver-cluster).

For more info, please see Chapter 6, "How to use Terraform as a Team", of 
*[Terraform: Up and Running](http://www.terraformupandrunning.com)*.

## Pre-requisites

* You must have [Terraform](https://www.terraform.io/) installed on your computer. 
* You must have [terragrunt](https://github.com/gruntwork-io/terragrunt) installed on your computer.
* You must have an [Amazon Web Services (AWS) account](http://aws.amazon.com/).
* You must deploy the MySQL database in [data-stores/mysql](../../data-stores/mysql) BEFORE deploying the
  templates in this folder.

Please note that this code was written for Terraform 0.7.x.

## Quick start

**Please note that this example will deploy real resources into your AWS account. We have made every effort to ensure 
all the resources qualify for the [AWS Free Tier](https://aws.amazon.com/free/), but we are not responsible for any
charges you may incur.** 

Configure your [AWS access 
keys](http://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) as 
environment variables:

```
export AWS_ACCESS_KEY_ID=(your access key id)
export AWS_SECRET_ACCESS_KEY=(your secret access key)
```

Fill in the name of an [S3](https://aws.amazon.com/s3/) bucket to use for remote state storage in `.terragrunt`:
 
```hcl
bucket = "(YOUR_BUCKET_NAME)"
``` 

In `vars.tf`, fill in the name of the S3 bucket and key where the remote state is stored for the MySQL database
(you must deploy the templates in [data-stores/mysql](../../data-stores/mysql) first):

```hcl
variable "db_remote_state_bucket" {
  description = "The name of the S3 bucket used for the database's remote state storage"
}

variable "db_remote_state_key" {
  description = "The name of the key in the S3 bucket used for the database's remote state storage"
}
```

Validate the templates:

```
terragrunt plan
```

Deploy the code:

```
terragrunt apply
```

Clean up when you're done:

```
terragrunt destroy
```
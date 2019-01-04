# Passing data between Terraform and Serverless with SSM Parameters

## Pre-requisites

1. Access to an AWS account.
1. `aws-cli` is installed and configured (see [AWS CLI docs](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) for specific configuration steps).
1. Terraform is installed (see [the Terraform website](https://www.terraform.io/) for instructions).
1. Serverless is installed (see the [Getting Started guide](https://serverless.com/framework/docs/getting-started/)).

## Deploying the infrastructure

1. Deploy the Terraform infrastructure:

```sh
cd terraform
terraform init
terraform apply
```

2. Deploy the Serverless project:

```sh
cd ../serverless
serverless deploy
serverless info --verbose | grep ServiceEndpoint | sed s/ServiceEndpoint\:\ //g # Gets Endpoint URL
```

3. Open `<ENDPOINT>?name=<VALUE_TO_ADD>` in your browser.

## Explanation

The `terraform` directory defines a small set of infrastructure elements: a VPC, the subnets, a security group, and an RDS MySQL database. The key there is the `parameters.tf` file that defines the SSM parameters and populates them with the information about the database.

The `serverless` directory contains a simple Serverless Framework project. The function invoked via an HTTP call takes a `name` query param. It inserts the param into the DB and renders the whole table. The function assumes that the table called `things` exists. There is no validation on the `name` param, so be creative!

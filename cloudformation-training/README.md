# AWS CloudFormation Training

Beginner-friendly AWS CloudFormation templates written in YAML.

## Files

1. `01-vpc.yaml` — Creates a VPC.
2. `02-subnet.yaml` — Creates a subnet inside the VPC.
3. `03-internet-gateway.yaml` — Creates and attaches an Internet Gateway.
4. `04-route-table.yaml` — Creates a route table, adds a default internet route, and associates it with the subnet.
5. `05-ec2.yaml` — Creates an EC2 instance and a security group allowing SSH from a specified trusted IP.

## Deployment order

Deploy each file as a separate CloudFormation stack, in this order:

1. `01-vpc.yaml`
2. `02-subnet.yaml`
3. `03-internet-gateway.yaml`
4. `04-route-table.yaml`
5. `05-ec2.yaml`

Use the same AWS Region for all stacks.

## Values to provide

- **Subnet stack:** select the VPC created by the first stack.
- **Internet Gateway stack:** select the same VPC.
- **Route table stack:** select the same VPC and subnet, and enter the Internet Gateway ID output by the third stack.
- **EC2 stack:** select the same VPC and subnet, provide an AMI ID valid in the selected Region, select an existing EC2 key pair, and enter your real public IPv4 address with `/32` for `SSHLocation`.

The IP address `203.0.113.10/32` is only an example/documentation address. Replace it with your own public IP address.

## Important networking note

`MapPublicIpOnLaunch: true` does not by itself make a subnet public. The route table must point `0.0.0.0/0` to the attached Internet Gateway, and the EC2 instance needs a public IPv4 address for direct internet access.

## Clean up

Cloud resources can incur charges. When finished, delete stacks in reverse dependency order:

1. EC2
2. Route table
3. Internet Gateway
4. Subnet
5. VPC

If CloudFormation refuses to delete a stack, check for resources still being used by another stack.

## Safety

The EC2 security group in this example allows SSH (port 22) only from the CIDR supplied for `SSHLocation`. Do not use `0.0.0.0/0` for SSH access in a real environment.

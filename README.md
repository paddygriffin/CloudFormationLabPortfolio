# CloudFormation VPC Lab

This project provisions a reusable AWS network with CloudFormation. It demonstrates
network segmentation, multi-AZ routing, private-subnet egress, and controlled
administrative access.

## Architecture

- One VPC with DNS support and DNS hostnames enabled.
- Two public subnets and two private subnets across the first two Availability Zones.
- An Internet Gateway and shared public route table for public subnets.
- One NAT Gateway and Elastic IP per Availability Zone for resilient private-subnet egress.
- A public EC2 instance and jump box in public subnet A.
- A private EC2 instance in private subnet A.
- Private SSH and ICMP access permitted only from the jump box security group.
- SSH to public hosts restricted to the administrator CIDR supplied at deployment.
- Amazon Linux 2023 resolved through the regional public SSM parameter.

The two NAT Gateways improve availability but incur AWS hourly and data-processing
charges. For a low-cost learning deployment, use a separate single-NAT variant and
document the availability trade-off rather than silently accepting the cost.

## Deploy

From the repository root, set an administrator CIDR to your current public IP in
`/32` form, then deploy:

```powershell
$adminCidr = "198.51.100.10/32"
aws cloudformation deploy `
	--template-file MyVPC.yml `
	--stack-name cloudformation-vpc-lab `
	--parameter-overrides KeyPair=<existing-key-pair> AdminCidr=$adminCidr
```

Retrieve the jump box address and private instance ID from the stack outputs:

```powershell
aws cloudformation describe-stacks `
	--stack-name cloudformation-vpc-lab `
	--query "Stacks[0].Outputs"
```

## Validation

Install and run `cfn-lint` before deployment:

```powershell
pip install cfn-lint
cfn-lint MyVPC.yml
```

Delete the stack after testing to avoid ongoing NAT Gateway and EC2 charges:

```powershell
aws cloudformation delete-stack --stack-name cloudformation-vpc-lab
```
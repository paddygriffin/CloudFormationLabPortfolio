# Architecture Decision Record (ADR)

## Title
VPC CloudFormation Template

## Context
We need to create a Virtual Private Cloud (VPC) using AWS CloudFormation to manage
our network infrastructure as code. The design should demonstrate the separation of
internet-facing and private workloads, provide private-subnet egress for updates,
and remain repeatable across AWS Regions.

## Decision
We will use an AWS CloudFormation template (`MyVPC.yml`) to define and deploy our VPC.
The template will include the following components:
- VPC
- Two public and two private subnets across two Availability Zones
- Internet Gateway
- One NAT Gateway per Availability Zone with private route tables
- Security groups with administrator CIDR restrictions and security-group references
- Public, private, and jump-box EC2 instances for connectivity demonstrations
- CloudFormation outputs for the deployed network and test instances

Amazon Linux 2023 is resolved through the regional public SSM parameter instead of
hardcoded AMI IDs. SSH remains available for this lab through an existing key pair,
but the administrator CIDR must be supplied at deployment and should normally be a
single trusted `/32` address.

## Status
Accepted

## Consequences
### Positive
- Automated and consistent VPC deployment
- Easier to manage and update network infrastructure
- Version control of infrastructure code
- Multi-AZ subnet and NAT design demonstrates availability considerations
- Private workloads can reach the internet for updates without accepting inbound connections
- Region-independent AMI selection and explicit deployment outputs

### Negative
- Initial learning curve for CloudFormation
- Potential complexity in managing large templates
- Two NAT Gateways and EC2 instances create ongoing AWS charges
- The public EC2 instance and jump box are lab demonstrations rather than a complete production application platform

## Alternatives Considered
1. **Manual VPC Creation**: This was rejected due to the lack of automation and potential for human error.
2. **Using Terraform**: CloudFormation was selected because this is an AWS-focused project and keeps the infrastructure definition within AWS tooling.
3. **One shared NAT Gateway**: Rejected for the primary design because a single NAT Gateway creates an Availability Zone dependency. It remains a reasonable cost-saving option for a short-lived lab.
4. **Public SSH from anywhere**: Rejected because it unnecessarily exposes administration. SSH is restricted to the supplied administrator CIDR.

## Related ADRs
- None

## References
- [AWS CloudFormation Documentation](https://docs.aws.amazon.com/cloudformation/index.html)
- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/index.html)

## Date
2023-10-05

## Author
Patrick Griffin

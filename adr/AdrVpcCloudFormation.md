# Architecture Decision Record (ADR)

## Title
VPC CloudFormation Template

## Context
We need to create a Virtual Private Cloud (VPC) using AWS CloudFormation to manage our network infrastructure as code. This will allow us to automate the deployment and management of our VPC, ensuring consistency and repeatability.

## Decision
We will use an AWS CloudFormation template (`VPC.yml`) to define and deploy our VPC. The template will include the following components:
- VPC
- Subnets (public and private)
- Internet Gateway
- NAT Gateway
- Route Tables
- Security Groups

## Status
Accepted

## Consequences
### Positive
- Automated and consistent VPC deployment
- Easier to manage and update network infrastructure
- Version control of infrastructure code

### Negative
- Initial learning curve for CloudFormation
- Potential complexity in managing large templates

## Alternatives Considered
1. **Manual VPC Creation**: This was rejected due to the lack of automation and potential for human error.
2. **Using Terraform**: This was a AWS project so Terraform was done after.

## Related ADRs
- None

## References
- [AWS CloudFormation Documentation](https://docs.aws.amazon.com/cloudformation/index.html)
- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/index.html)

## Date
2023-10-05

## Author
Patrick Griffin

# CIDR / Subnet Plan

| Resource | Name | ID | CIDR | AZ | Auto-assign public IP |
|---|---|---|---|---|---|
| VPC | w1-vpc | vpc-01f0303486a330ed0 | 10.20.0.0/16 | - | - |
| Public subnet | w1-public-a | subnet-070f3886171fb149d | 10.20.1.0/24 | ap-south-1a | Yes |
| Public subnet | w1-public-b | subnet-0ad3285e3b8034f96 | 10.20.2.0/24 | ap-south-1b | Yes |
| Private subnet | w1-private-a | subnet-0ec8ff7bcc57882ed | 10.20.11.0/24 | ap-south-1a | No |
| Private subnet | w1-private-b | subnet-0eaf7fdbf0a0f3e3d | 10.20.12.0/24 | ap-south-1b | No |

All subnet CIDRs are inside 10.20.0.0/16 and do not overlap. Each tier spans two AZs.

Evidence: screenshots 01 to 05.

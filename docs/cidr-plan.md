# CIDR / Subnet Plan

| Resource | Name | CIDR | AZ | Auto-assign public IP |
|---|---|---|---|---|
| VPC | w1-vpc | 10.20.0.0/16 | - | - |
| Public subnet | w1-public-a | 10.20.1.0/24 | ap-south-1a | Yes |
| Public subnet | w1-public-b | 10.20.2.0/24 | ap-south-1b | Yes |
| Private subnet | w1-private-a | 10.20.11.0/24 | ap-south-1a | No |
| Private subnet | w1-private-b | 10.20.12.0/24 | ap-south-1b | No |

All subnet CIDRs are inside 10.20.0.0/16 and do not overlap each other. Each tier spans two AZs for availability.

# Route Tables

## w1-public-rt (associated: w1-public-a, w1-public-b)
| Destination | Target |
|---|---|
| 10.20.0.0/16 | local |
| 0.0.0.0/0 | igw-xxxxxxxx (w1-igw) |

## w1-private-rt (associated: w1-private-a, w1-private-b)
| Destination | Target |
|---|---|
| 10.20.0.0/16 | local |

No route to the Internet Gateway. Private subnets are not directly reachable from the internet.

## Routing path per subnet
- **Public subnets:** instance -> subnet -> w1-public-rt -> IGW -> Internet (and the reverse for public IP traffic).
- **Private subnets:** instance -> subnet -> w1-private-rt -> local only. VPC-internal traffic only.

## Outbound internet for private instances
- **Preferred (needs approval, chargeable):** NAT Gateway in a public subnet with an Elastic IP; add `0.0.0.0/0 -> nat-xxxx` to w1-private-rt. Production uses one NAT per AZ.
- **Low cost partial option:** S3 Gateway VPC Endpoint (free) for S3-hosted repositories.
- **This assignment:** NAT was not approved, so private subnets stay isolated intentionally, which demonstrates private instance isolation.

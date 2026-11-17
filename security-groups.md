# Security Groups (least privilege)

## w1-public-sg
| Direction | Protocol/Port | Source/Dest | Reason |
|---|---|---|---|
| Inbound | TCP 80 | 0.0.0.0/0 | Web test page (optional) |
| Inbound | TCP 22 | _<my-ip>/32_ or none (use Session Manager) | Admin access, never 0.0.0.0/0 |
| Outbound | All | 0.0.0.0/0 | Default |

## w1-private-sg
| Direction | Protocol/Port | Source/Dest | Reason |
|---|---|---|---|
| Inbound | ICMP | w1-public-sg | Ping test from public instance |
| Inbound | TCP 22 | w1-public-sg | Controlled bastion access |
| Inbound | TCP 80 | w1-public-sg | App test |
| Outbound | All | 0.0.0.0/0 | Default (no route out without NAT) |

No SSH/RDP is exposed to the entire internet. Private SG references the public SG instead of CIDRs.

## IDs
- w1-public-sg: `sg-04571e3a4917f16e9`
- w1-private-sg: `sg-058378607d87f1882`

Both are in `vpc-01f0303486a330ed0` (w1-vpc). Evidence: screenshot 11. _Add a screenshot of each group's Inbound rules tab._

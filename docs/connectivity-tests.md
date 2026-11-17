# Connectivity Test Evidence

Fill the Actual column and attach screenshots (screenshots/test-N.png).

| # | Source | Destination | Protocol:Port | Command | Expected | Actual | Screenshot |
|---|---|---|---|---|---|---|---|
| 1 | My browser | w1-public-ec2 public IP | TCP:80 | open http://<ip> | Success | | test-1.png |
| 2 | w1-public-ec2 | example.com | TCP:443 | `curl -I https://example.com` | Success (IGW) | | test-2.png |
| 3 | w1-public-ec2 | DNS (10.20.0.2) | UDP:53 | `nslookup amazon.com` | Resolves | | test-3.png |
| 4 | Random internet IP | w1-public-ec2 | TCP:22 | `ssh ec2-user@<ip>` | Blocked (timeout) | | test-4.png |
| 5 | w1-public-ec2 | w1-private-ec2 | ICMP / TCP:22,80 | `ping <private-ip>` | Allowed (SG ref) | | test-5.png |
| 6 | Internet | w1-private-ec2 | any | no public IP | Unreachable | | test-6.png |
| 7 | w1-private-ec2 | example.com | TCP:443 | `curl -m 5 https://example.com` | Blocked (no NAT) | | test-7.png |
| 8 | w1-private-ec2 | DNS | UDP:53 | `nslookup amazon.com` | Resolves, but no connectivity | | test-8.png |

## Troubleshooting demo
1. Delete `0.0.0.0/0 -> igw` from w1-public-rt.
2. Run test 2: fails.
3. Re-add route: test 2 passes again.

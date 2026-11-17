# Connectivity Test Evidence

Instance: `w1-public-ec2` (`i-0921544ff40e0f247`), subnet w1-public-a, private IP 10.20.1.112, public IP 13.126.147.245 (temporary; instance is terminated after assessment).

| # | Source | Destination | Protocol:Port | Command / method | Expected | Actual | Evidence |
|---|---|---|---|---|---|---|---|
| 1 | My browser | w1-public-ec2 public IP | TCP:80 | http://13.126.147.245 | Success | **Pass**: page "Task1 Successfully Complete" shown | screenshots/15-test-web-page.png |
| 2 | w1-public-ec2 | Internet | TCP:443 | `curl -s https://checkip.amazonaws.com` | Success (IGW) | **Pass**: returned 13.126.147.245 | terminal output (add screenshot) |
| 3 | w1-public-ec2 | VPC DNS resolver | UDP:53 | `nslookup amazon.com` | Resolves | Pending | |
| 4 | Random internet IP | w1-public-ec2 | TCP:22 | `ssh ec2-user@<ip>` | Blocked (timeout) | Pending | |
| 5 | w1-public-ec2 | w1-private-ec2 | ICMP, TCP:22/80 | `ping <private-ip>` | Allowed (SG ref) | Pending (private instance) | |
| 6 | Internet | w1-private-ec2 | any | no public IP | Unreachable | Pending (private instance) | |
| 7 | w1-private-ec2 | Internet | TCP:443 | `curl -m 5 https://example.com` | Blocked (no NAT) | Pending (private instance) | |
| 8 | w1-private-ec2 | VPC DNS resolver | UDP:53 | `nslookup amazon.com` | Resolves, no connection | Pending (private instance) | |

## Additional checks on the public instance
- `systemctl status httpd`: active (running), listening on port 80.
- `curl -I localhost`: HTTP/1.1 200 OK.

## Troubleshooting demo
1. Delete `0.0.0.0/0 -> igw` from w1-public-rt.
2. Run test 2: fails.
3. Re-add the route: test 2 passes again.

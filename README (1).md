# Week 1: Custom AWS Network (VPC)

**IT-SIMPLERA SOLUTIONS | Cloud Computing AWS**
**Student:** _<your name>_  |  **Submission date:** _<date>_

## 1. Objective
Design and implement a segmented AWS network with two Availability Zones, public and private subnets, controlled routing and least-privilege security groups, and verify it with allowed and intentionally blocked connectivity tests.

## 2. Architecture
![Architecture](diagrams/architecture.svg)

```mermaid
flowchart TB
  Internet((Internet)) --> IGW[Internet Gateway w1-igw]
  subgraph VPC["w1-vpc 10.20.0.0/16 (ap-south-1)"]
    subgraph AZa["ap-south-1a"]
      PUBA["w1-public-a 10.20.1.0/24"]
      PRIVA["w1-private-a 10.20.11.0/24"]
    end
    subgraph AZb["ap-south-1b"]
      PUBB["w1-public-b 10.20.2.0/24"]
      PRIVB["w1-private-b 10.20.12.0/24"]
    end
  end
  IGW --> PUBA
  IGW --> PUBB
  PUBA -. SG reference .-> PRIVA
```

**Region choice:** ap-south-1 (Mumbai): low latency from Pakistan, 3 AZs, reasonable pricing.

## 3. Prerequisites
- AWS account with VPC / EC2 / IAM permissions
- AWS Console access (optionally Terraform >= 1.5)
- Instructor approval before creating chargeable resources (NAT Gateway, extra public IPv4)

## 4. Design summary
| Item | Value |
|---|---|
| VPC | w1-vpc `vpc-01f0303486a330ed0`, 10.20.0.0/16 |
| Subnets | 2 public + 2 private across 2 AZs ([docs/cidr-plan.md](docs/cidr-plan.md)) |
| Routing | Public RT -> IGW; Private RT -> local only ([docs/route-tables.md](docs/route-tables.md)) |
| Security | Least-privilege SGs ([docs/security-groups.md](docs/security-groups.md)) |
| Access | Session Manager (no SSH open to the internet) |
| Private outbound | NAT not approved, so private subnets are isolated by design |

## 5. Deployment steps
1. Create VPC `w1-vpc` (10.20.0.0/16); enable DNS resolution and DNS hostnames.
2. Create 4 subnets per the CIDR plan; enable auto-assign public IPv4 on public subnets only.
3. Create and attach Internet Gateway `w1-igw`.
4. Create `w1-public-rt` with `0.0.0.0/0 -> igw`; associate public subnets only.
5. Create `w1-private-rt` (local route only); associate private subnets.
6. Create `w1-public-sg` and `w1-private-sg`.
7. Create IAM role `w1-ssm-role` (AmazonSSMManagedInstanceCore).
8. Launch `w1-public-ec2` in `w1-public-a`; optionally `w1-private-ec2` in `w1-private-a`.
9. Run the tests in [docs/connectivity-tests.md](docs/connectivity-tests.md).

Optional: reproduce the network with Terraform ([terraform/](terraform/)).

## 6. Evidence (screenshots)
Account IDs are masked in all screenshots.

### VPC
<img width="1201" height="560" alt="01-vpc" src="https://github.com/user-attachments/assets/8aaaa952-86da-4d2d-8f8f-94203a229afa" />

### Subnets
| Public A | Public B |
|---|---|
| <img width="1143" height="486" alt="02-subnet-public-a" src="https://github.com/user-attachments/assets/e43df5d9-009c-4448-998c-5a7eebe21a8b" />  | <img width="1131" height="481" alt="03-subnet-public-b" src="https://github.com/user-attachments/assets/8775f5be-1d75-49c6-8dae-42904b683ab1" />
 |

| Private A | Private B |
|---|---|
| <img width="1092" height="476" alt="04-subnet-private-a" src="https://github.com/user-attachments/assets/1e359d9b-2075-4044-a6ca-17394b44223d" />  | <img width="1067" height="477" alt="05-subnet-private-b" src="https://github.com/user-attachments/assets/f8f28723-e8b6-4eaa-8cda-0f1161f7d334" />
 |

### Internet Gateway
![IGW](screenshots/06-internet-gateway.png)

### Route tables
Public route table (creation and explicit subnet associations):

![](screenshots/07-public-rt-create.png)
![](screenshots/08-public-rt-associations.png)

Private route table (creation and routes: `10.20.0.0/16 -> local` only):

![](screenshots/09-private-rt-create.png)
![](screenshots/10-private-rt-routes.png)

### Security groups
![SGs](screenshots/11-security-groups.png)

### IAM role for Session Manager
![](screenshots/12-iam-role-policy.png)
![](screenshots/13-iam-role-review.png)

### EC2 instance and web test
![](screenshots/14-ec2-public-instance.png)
![](screenshots/15-test-web-page.png)

## 7. Test results
Summary below; details in [docs/connectivity-tests.md](docs/connectivity-tests.md).

| # | Test | Result |
|---|---|---|
| 1 | Browser -> public EC2, TCP 80 | Pass |
| 2 | Public EC2 -> internet, TCP 443 | Pass |
| 3-8 | DNS, blocked SSH, private isolation | Pending |

## 8. Troubleshooting scenario
The web page initially did not load although Apache was running (`curl localhost` returned 200 and Apache showed zero requests). `curl https://checkip.amazonaws.com` on the instance returned the public IP, proving the IGW and public route worked for outbound traffic, which narrowed the fault to inbound access. After correcting the inbound path, the page loaded (screenshot 15).

## 9. Cleanup
See [docs/cleanup.md](docs/cleanup.md).

## 10. Security notes
No keys, credentials or account IDs are committed. `.gitignore` blocks `*.pem`, `*.key`, `.env` and Terraform state.

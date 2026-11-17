# Cleanup Record

Order of deletion (tick and attach screenshot/date):

- [ ] Terminate EC2 instances (w1-public-ec2, w1-private-ec2)
- [ ] Delete NAT Gateway (if created) and release Elastic IP
- [ ] Delete VPC endpoints (if created)
- [ ] Detach and delete Internet Gateway
- [ ] Disassociate and delete custom route tables
- [ ] Delete subnets
- [ ] Delete security groups
- [ ] Delete VPC
- [ ] Delete IAM role w1-ssm-role
- [ ] Check Billing dashboard: no chargeable resources remain

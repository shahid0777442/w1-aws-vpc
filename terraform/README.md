# Terraform (optional)
```
terraform init
terraform plan -var="admin_cidr=<your-ip>/32"
terraform apply -var="admin_cidr=<your-ip>/32"
# ... tests ...
terraform destroy
```

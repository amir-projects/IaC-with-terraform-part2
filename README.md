# 🚀 Terraform for Absolute Beginners  
### Learn by Doing — Step-by-Step with Examples

Welcome! 🙌  
This project is designed for beginners with **zero experience in Terraform**.  
You’ll learn some important Terraform topics and then try a **hands-on lab** to create a resource in AWS.

---

## 📚 What You'll Learn

1. ✅ Terraform Variables
2. ✅ Terraform Outputs
3. ✅ Terraform Dynamic Blocks
4. ✅ Terraform Format & Import
5. ✅ Terraform Debugging
6. ✅ Hands-on Lab: Create & Tag an AWS S3 Bucket

---

## 1️⃣ Terraform Variables

Variables help you avoid hardcoding values.  
Instead of writing the same thing again and again, you define a variable and use it.

📄 `variables.tf`
```hcl
variable "region" {
  default = "us-west-2"
}

variable "bucket_name" {
  type        = string
  description = "The name of the S3 bucket"
}

```sh
You can provide values like this when running Terraform:
terraform apply -var="bucket_name=my-demo-bucket"
```
2️⃣ Terraform Outputs
Outputs show you important information after Terraform runs — such as an S3 bucket's ARN, public IP, etc.

📄 outputs.tf

output "bucket_arn" {
  value = aws_s3_bucket.demo.arn
}
When you run terraform apply, the ARN will be printed.

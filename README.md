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

## 1️⃣ Terraform Variables - Making your code resuable 
   🎯 Goal: We want to store values like region and bucket name in one place — so we can reuse and change them easily later
   🧠 Why use variables?
      Imagine you are writing code to create a bucket. If you hardcode the bucket name like this:
      bucket = "student-lab-bucket"
      Then, if 10 people use this code — it will fail due to duplicate bucket names!
   ✅ Solution: Use a variable so each person can give a unique name without touching the code.
   📘 What are Variables?
   - Variables allow you to pass values into your Terraform code, making it reusable and flexible.
   - Variables help you avoid hardcoding values.  
   - Instead of writing the same thing again and again, you define a variable and use it.

📄 `variables.tf`
```hcl
variable "region" {
description = "AWS Region"
default = "us-west-2"
}

variable "bucket_name" {
  type        = string
  description = "The name of the S3 bucket"
}
```
📌 This says:
- Region is fixed to us-west-2
- Bucket name must be passed from the user

🧾Create a file called `terraform.tfvars`
```bash
terraform apply -var="bucket_name=my-demo-bucket"
```

---
📌 This file gives value to bucket_name so Terraform doesn’t ask for it during execution.

## 2️⃣ Terraform Outputs

Outputs show you important information after Terraform runs — such as an S3 bucket's ARN, public IP, etc.

📄 `outputs.tf`
```hcl
output "bucket_arn" {
  value = aws_s3_bucket.demo.arn
}
```

When you run `terraform apply`, the ARN will be printed.

---

## 3️⃣ Terraform Dynamic Blocks

Dynamic blocks let you write repeating blocks like tags, rules, or configs.

📄 Example with tags:
```hcl
variable "tags" {
  type = map(string)
  default = {
    Owner       = "TeamA"
    Environment = "Dev"
  }
}

resource "aws_s3_bucket" "demo" {
  bucket = var.bucket_name
  tags   = var.tags
}
```

---

## 4️⃣ Terraform Format & Import

### 🧹 Format

To fix spacing, indentation, and make code look nice:
```bash
terraform fmt
```

### 🔄 Import

If you already have a resource in AWS (e.g., S3 bucket), and want to manage it with Terraform:
```bash
terraform import aws_s3_bucket.demo existing-bucket-name
```

---

## 5️⃣ Terraform Debugging

If Terraform fails or behaves unexpectedly, you can turn on debugging.

To show logs in terminal:
```bash
TF_LOG=DEBUG terraform apply
```

To save logs to a file:
```bash
TF_LOG=DEBUG TF_LOG_PATH=debug.log terraform plan
```

---

## 6️⃣ 🧪 Hands-on Lab: Create a Tagged S3 Bucket

Let’s build a real AWS resource using everything you’ve learned.

📄 `main.tf`
```hcl
provider "aws" {
  region = var.region
}

resource "aws_s3_bucket" "demo" {
  bucket = var.bucket_name
  tags   = var.tags
}
```

📄 `variables.tf`
```hcl
variable "region" {
  default = "us-west-2"
}

variable "bucket_name" {
  type = string
}

variable "tags" {
  type = map(string)
  default = {
    Project = "TerraformDemo"
    Owner   = "BeginnerUser"
  }
}
```

📄 `outputs.tf`
```hcl
output "bucket_arn" {
  value = aws_s3_bucket.demo.arn
}
```

---

## ▶️ How to Run This Lab

1. Open your terminal.
2. Go to this folder using `cd path-to-folder`.
3. Run the following:

```bash
terraform init
terraform plan -var="bucket_name=my-first-bucket"
terraform apply -var="bucket_name=my-first-bucket"
```

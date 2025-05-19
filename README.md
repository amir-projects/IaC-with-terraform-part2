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

## 🟢 STEP -1 Terraform Variables - Making your code resuable 
   🎯 Goal: We want to store values like region and bucket name in one place — so we can reuse and change them easily later

   🧠 Why use variables?
   
      Imagine you are writing code to create a bucket. If you hardcode the bucket name like this:
      bucket = "student-lab-bucket"
      Then, if 10 engineers use this code — it will fail due to duplicate bucket names!
      
   ✅ Solution: Use a variable so each person can give a unique name without touching the code.
 

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
bucket_name = "my-lab-bucket-002"
```

---
📌 This file gives value to bucket_name so Terraform doesn’t ask for it during execution.

✅ Quick Recap:
- You defined placeholders (region, bucket_name) using variables.
- You gave real values to those placeholders in terraform.tfvars.

🟢 STEP 2: Create an AWS S3 Bucket Using Those Variables
 🎯 Goal:Use the variables to create a real S3 bucket in AWS.

🧾`Create a file called main.tf`:

```bash 
provider "aws" {
  region = var.region
}

resource "aws_s3_bucket" "lab_bucket" {
  bucket = var.bucket_name
}
```
📌 This does two things:

- Tells Terraform to use AWS and the region from var.region

- Creates a new S3 bucket using the name from var.bucket_name
So when you run:
```bash
terraform init
terraform apply
```
Terraform reads:

- variables.tf to know what values are needed

- terraform.tfvars to get the real values

- main.tf to apply them to AWS

🧪 Output:
- A bucket will be created in AWS S3
- The bucket name will match what you gave in terraform.tfvars


## 2️⃣ Terraform Outputs - See What you made
🎯 Goal: After creating something (like a bucket), we want to see and use its name, ARN, or URL easily — that's what outputs do.

📄 `outputs.tf`
```hcl
output "bucket_name" {
  value = aws_s3_bucket.lab_bucket.bucket
}
```
📌 This prints the bucket name after apply.

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

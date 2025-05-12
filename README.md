# terraform_notes

🌐 **What is Terraform?** <br>
Terraform is an open-source **Infrastructure as Code (IaC)** tool developed by **HashiCorp**. It allows you to define and provision infrastructure using a high-level **configuration language (HCL - HashiCorp Configuration Language).**

<hr>

🧱 **Key Features**
 - Infrastructure as Code – Automate infrastructure setup.
 - Idempotency – Running the same script results in the same infrastructure.
 - Execution Plan – Preview changes before applying them.
 - State Management – Keeps track of infrastructure state.
 - Provider Support – Works with AWS, Azure, GCP, etc.

<hr>

🔧 **Terraform Core Concepts**
1. Providers
Responsible for managing the lifecycle of resources (e.g., AWS, Azure, GCP).
```ssh
provider "aws" {
  region = "us-west-2"
}
```

2. Resources
Describes the infrastructure components.
```ssh
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

3. Variables
Used for parameterizing configurations.
```ssh
variable "region" {
  default = "us-west-2"
}
```

4. Outputs
Return values from a Terraform module or configuration.
```ssh
output "instance_ip" {
  value = aws_instance.example.public_ip
}
```

5. Modules
Reusable and composable units of configuration.
```ssh
module "network" {
  source = "./network"
}
```

<hr>

🔁 **Terraform Workflow** <br>
1. *terraform init*      - Downloads required providers and sets up the working directory <br>
2. *terraform plan*      - Previews the changes that will be made to the infrastructure <br>
3. *terraform apply*     - Executes the planned changes to create/update resources <br>
4. *terraform destroy*   - Destroys the infrastructure created by Terraform <br>

<hr>

🗃 **Terraform State** <br>
 - Terraform stores information about the infrastructure in a state file **(terraform.tfstate)**. <br>
 - This allows Terraform to track changes over time.  <br>
 - Remote backends (S3, Azure Blob, GCS) are recommended for team use. <br>

 Example backend config: <br>
 ```ssh
 terraform {
  backend "s3" {
    bucket = "my-tf-state"
    key    = "dev/terraform.tfstate"
    region = "us-west-2"
  }
 }
```

<hr>

📦 **Modules** <br>
 - Root Module: The main working directory with **.tf** files. <br>
 - Child Modules: Called from other modules, can be reused. <br>

 Example: <br>
 ```ssh
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  version = "3.0.0"
}
```

<hr>

📘 **Example Project Structure**
```ssh
terraform/
│
├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
├── modules/
│   └── vpc/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
```

<hr>


**Day 01 With Terraform**

*Creation of EC2 Instance*

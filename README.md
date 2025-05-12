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

*Step 1 : Terraform Installation on EC2 Instance* <br>

```ssh
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```

 - *git clone* - git clone <link> download code from windows to EC2 machine.  <br>
 
 - *terraform init* - Initialize the backend, Install provider plugins, Configure the working directory. <br>
 
 - *terraform plan* - The terraform plan command is used to preview the changes Terraform will make to your infrastructure based on the current configuration files and the existing state. <br>
 
 - *terraform apply* - The terraform apply command is used to execute the changes defined in your Terraform configuration—provisioning, updating, or destroying infrastructure as needed. <br>
 
 - *terraform destroy* - The terraform destroy command is used to delete all the infrastructure that was created using your Terraform configuration. It is the reverse of terraform apply. <br>

*Step 2 : Creation of Simple EC2 Instance*  <br>

**first.tf :**

```ssh
provider "aws" {
  region = "ap-south-1" # or your preferred region
}

resource "aws_instance" "web" {
  ami           = "ami-0e35ddab05955cf57"
  instance_type = "t2.micro"

  tags = {
    Name = "MyWebServer"
  }
}
```

<hr>

**Day 02 With Terraform**

*Added terraform, varibale and output block in the first.tf file*

**first.tf :**

*To store files in centralized location i.e s3 bucket*
```ssh
terraform {
  backend "s3" {
    bucket = "s3-for-terraform-demo"
    key    = "new-key.pem"
    region = "ap-south-1"
  }
}
```

*added input variables*
```ssh
terraform {
  backend "s3" {
    bucket = "s3-for-terraform-demo"
    key    = "new-key.pem"
    region = "ap-south-1"
  }
}

provider "aws" {
  region = "ap-south-1" 
}

variable "instance_type" {
  description = "This is the instance type for the demo EC2 instance"
  type        = string
  default     = "t3.micro"
}

resource "aws_instance" "web" {
  ami  = "ami-0e35ddab05955cf57"
  instance_type = var.instance_type

  tags = {
    Name = "MyWebServer"
  }
}
```

*added output variable*

```ssh
terraform {
  backend "s3" {
    bucket = "s3-for-terraform-demo"
    key    = "new-key.pem"
    region = "ap-south-1"
  }
}

provider "aws" {
  region = "ap-south-1" 
}

variable "instance_type" {
  description = "This is the instance type for the demo EC2 instance"
  type        = string
  default     = "t3.micro"
}

output "instance_ip_addr" {
  value = aws_instance.web.private_ip
}


resource "aws_instance" "web" {
  ami  = "ami-0e35ddab05955cf57"
  instance_type = var.instance_type

  tags = {
    Name = "MyWebServer"
  }
}
```

*//Output :*

![Alt text](Screenshot 2025-05-12 142312)



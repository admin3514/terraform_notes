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
1. terraform init      - Downloads required providers and sets up the working directory <br>
2. terraform plan      - Previews the changes that will be made to the infrastructure <br>
3. terraform apply     - Executes the planned changes to create/update resources <br>
4. terraform destroy   - Destroys the infrastructure created by Terraform <br>


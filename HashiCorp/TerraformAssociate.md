# Terraform Associate Certification Exam

[Exam Review](https://learn.hashicorp.com/tutorials/terraform/associate-review)

## Exam Overview

### Exam description

- 1 hour
- 50 to 60 questions, v0.12+
- Online Proctored
- 2 years validity period

### Exam Objectives (9)

1. Understand Infrastructure as Code (IaC) concepts
   - Explain/define IaC
   - Describe advantages of IaC
2. Understand Terraform's purpose (vs other IaC)
   - Know what Terraform state is
   - Know Terrafrom's multi-cloud benefits
3. Understand Terraform basics
   - Install Terraform
   - Use Terraform providers
   - Use Terraform provisioners
4. Use the Terraform CLI (outside of core workflow)
   - How to debug Terraform
   - Terraform fmt, state, taint, import, and workspaces
5. Interact with Terraform modules
   - Differentiate module sources
   - Module inputs and outputs and module versioning
   - Variable scope within root/child modules, public registry
6. Navigate Terraform worklfow
   - Local & remote state storage, state locking
   - Terraform backend block and Terraform refresh effects
   - Backend auth and secret management in state files
7. Implement and maintain state
   - Describe Terraform workflow
   - Initialize Terraform working dir (terraform init)
   - Terraform validate, plan, apply, destroy
8. Read, generate, and modify configuration
   - Terraform variables and outputs
   - Create and contrast resource and data blocks
   - Resource addressing, built-in functions & dynamic blocks
9.  Understand Terraform Cloud and Enterprise Capabilities
    - Describe benefitrs of **Sentinel**, registry and workspaces
    - Differentiate **Terraform OSS** and **Enterprise workspaces**
    - Summarize features of Terraform Cloud offering

### Question Types

- **True/False** - E.g. Unernames and passwords referenced in the Terraform code, even as variables, will end up in plain text in the state file. True or False?
  - True
- **Multiple Choice Single Answer** - E.g. Consider the following Terraform 0.12 configuration snippet
  ```json
  variable "vpc_cidrs" {
    type = map
    default = {
       us-east-1 = "10.0.0.0/16"
       us-east-2 = "10.1.0.0/16"
       us-west-1 = "10.2.0.0/16"
       us-west-2 = "10.3.0.0/16"
    }
  }
  resource "aws_vpc" "shared" {
    cidr_block = ???????
  }
  ``` 
  How would you define the cidr_block for us-east-1 in the aws_vpc resource using a variable?
  - var.vpc_cidrs["us-east-1"] V
  - var.vpc_cidrs.0 X
  - vpc_cidrs["us-east-1"] X
  - var.vpc_cidrs[0] X
- **Multiple Choice Multiple Answer** - E.g. Which of the following Terraform commands will automatically refresh the state unless supplied with additional flags or arguments? Choose TWO correct answers.
  - terraform plan V
  - terraform state X
  - terraform apply V
  - terraform validate X
  - terraform output X
- **Text Match/Fill-in the blank** - E.g. Which *flag* us used to find information about a Terraform command? For example, you need additional information about how to use the *plan* command. You would type *terraform plan ?????*. Type your answer in the fireld provided. The text field is not case-sensitive and all variations of the correct answer are accepted.
  - h V
  - help V
  - --help V

### Certification benefits

- Verify your skills as an IaC practitioner
- Expand your skills and knowledge about Terraform and its best practices
- Get insights into the Enterprise features of Terraform
- Solidify Terraform fundamentals and know the basics for more advanced deployments

Other IaC tools: Ansible, Chef, Puppet

## Understanding IaC

### IaC and Its Benefits

What is an IaC?

- No more clicks - write down what you want to deploy in a human readable way
- DevOps enabler - codification of deployments means they can be tracked down in version control enabling better visibility and collaboration across teams, and additionaly code acts as an infra documentation
- Declare your infrastructure - infra is mostly written using declarative code but can be procedural(imperative) too
- Speed, reduced costs and risk - less human intervention during deployment reduces probability of securety flaws, unnecessary resources and saves time

Terraform code sample

- HCL (HashiCorp Configuration Language)
- Terraform code is easy to read
- Allows for  use of variables ang logic control

```h
# Declare provider - AWS
provider "aws" {}

# Create resource - AWS VPC in us-east-1
resource "aws_vpc" "vpc_master" {
    cidr_block = "10.0.0.0/16"
    enable_dns_support = true
    enable_dns_hostnames = true
    tags = {
        Name = "master-vpc-jenkins"
    }
}
```

### Cloug Agnostic IaC with Terraform

Terraform benefits
- Automate software defined networking (SDN - AWS VPC etc.)
- Interacts and takes care of communication with control layer APIs with ease
- Supports a vast array of providers for private and public cloud vendors
   - major cloud (AWS, Azure, GCP)
   - cloud (DigitalOcean, Fastly, Heroku)
   - network, infrastructure software, database etc.
- Tracks state of each resource deployed

### Understanding IaC - Recap

**Cloud agnostic** = technology not bound to one cloud and can work in a similar fashion across other cloud environments. Terraform doesn't care what cloud or infrastructure deployment method you're using and it works seamlessly with a long (and growing) list of cloud platforms.

IaC **enables** organizations **to have better DevOps practices by tracking infrastructure code and deploying it in a repeatable, predictable manner**. IaC is a great way to enable quick deployments and collaboration across teams.

Terraform uses its own language known as **HashiCorp Configuration Language (HCL)**.

IaC is a **method** of **writing human-readable code to deploy resources in the cloud and elsewhere**. IaC = code that deploys your infrastructure resources onto various platforms instead of managing them manually through a user interface.

## IaC With Terraform

### What is the Terraform Workflow

The Core Terraform Workflow: Write > Plan > Apply

Write - write your Terraform code - this would generally start off with creating a GitHub repo as a common best practice
Plan  - continually add and review changes to code in your project
Apply - after final review/plan, deploy/provision real infrastructure

### Terraform Init (Initializing the Working Directory)

**terraform init** - initializes the working directory that contains your Terraform code
   - **Downloads ancillary components** - providers, modules and plugins
   - **Sets up the backend for storing Terraform state file**, a mechanism by which Terraform tracks resources

### Terraform Key Conecpts: Plan, Apply, and Destroy

Terraform Core Workflow: Write Code > Plan/Review > Deploy/Apply

#### Terraform Plan

- Reads the code and then creates and shows a "plan" of execution/deployment; this command does not deploy anything and can be considered as **a read-only command**
- Allows you to review the action plan before executing anything
- At this stage authentication credentials are used to connect to your infrastructure, if required

#### Terraform Apply

- Deploys the instructions and statements in the code
- Updates the **deployment state tracking mechanism** file, aka **"state file"** - terraform.tfstate

#### Terraform Destroy

- Looks at the recorded, stored state file created during deployment and destroys all resources created by toyr code
- Should be used with caution, as it is **a non-reversible command**; take backups, and be sure that you want to delete infrastructure

### Resource Addressing in Terraform

Terraform Code

- Terraform executes code in files with the .tf extension
- By default, terraform looks for providers in the **Terraform providers registry** - https://registry.terraform.io/browse/providers, but providers can also be sourced locally or internally
- Custom providers can be created

```T
### Provider configuration block sample
provider "aws" {
   region = "us-east-1"
}
# provider - reserved keyword
# "aws" - name of the provider
# {} - configuration parameters, which vary based on provider you use

# Google provider example
provider "google" {
   credentials = file("credentials.json")
   project = "my-gcp-project"
   region = "us-west-1"
}
# file - built-in function

### Resource block sample
resource "aws_instance" "web" {
   ami           = "ami-a1b2c3d4"
   instance_type = "t2.micro"
}
# resource - reseved keyword, creates resource
# "aws_instance" - resource provided by Terraform provider
# "web" - user-provided arbitrary resource name
# {} - resource config arguments, depend on resource you create
# resource address = aws_instance.web

### Data source block sample
data "aws_instance" "my_vm" {
   instance_id = "i-1234567890abcdef0"
}
# Fetches data from already existing resource
# data - reserved keyword
# resource provided by Terraform provider
# "my_vm" - user-provided arbitrary resource name
# {} - data source arguments
# resource address = data.aws_instance.my-vm
```

### Deploying a VM in AWS Using the Terraform Workflow

- Create a directory and write Terraform code (Write)
- Plug the provided AMI and subnet ID values into your code
- Initialize and review your Terraform code (Plan)
- Deploy your Terraform code (Apply), verify your resources and clean up

```T
provider "aws" {
  region = "us-east-1"
}
resource "aws_instance" "vm" {
  ami           = "DUMMY_VALUE_AMI_ID"
  subnet_id     = "DUMMY_VALUE_SUBNET_ID"
  instance_type = "t3.micro"
  tags = {
    Name = "my-first-tf-node"
  }
}
```

### IaC With Terraform - Recap

- Terraform supports most major cloud providers, with a growing list of common and uncommon cloud providers
- Terraform workflow - **Write > Plan > Apply** - the recommended Terraform workflow is to write the code (Write), review it (Plan), and then execute/deploy the code (Apply)

- Benefits of using Terraform as an IaC tool:
  - Tracks state of each resource deployed and allows for a consistent deployment each time.
  - Interacts and takes care of the communication with control-layer APIs with ease
  - Automate software-defined networks deployments

- **terraform init** command
   - initializes a working directory containing Terraform configuration files
   - configures and sets up the backend which will store the state file
   - the first command that should be executed after writing a new Terraform configuration or cloning an existing one from version control
   - it is safe to run this command multiple times
   - more information: [Commmand: init](https://developer.hashicorp.com/terraform/cli/commands/init) 
- **terraform apply** deploys resources into real environments and tracks them through a state file
- **terraform plan** command
   - goes through the code and creates a plan of execution on which the apply command acts
   - outputs a plan of the actions that will be taken during deployment for review prior to execution
- **terraform destroy** 
   - destructive command which deletes all resources being tracked via the Terraform state file
   - cleans up and deletes all infrastructure tracked in the state file

## Terraform Fundamentals

### Installing Terraform and Terraform Providers

#### Method 1: Download, unzip, use

- Download OS specific zipped binary from [HashiCorp web site](https://developer.hashicorp.com/terraform/downloads?product_intent=terraform)
- Unzip Terraform binary
- Add path to Terraform binary into the system's $PATH variable

```Bash
wget -c <URL>
unzip <FILENAME>
# add path to $PATH var or move executable into a directory which is already added into $PATH (e.g. /bin)
terraform --version
```

#### Method 2: Set up Terraform reposiotory (Linux)

- Add/configure a HashiCorp Terraform repo on Linux (Debian, RHEL, Amazon Linux)
- Use package manager to install Terraform
- Package manager installs and sets up Terraform making it ready to use

```Bash
# CentOS/RHEL
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum -y install terraform
terraform --version
terraform -help
```

#### Terraform Providers

- Providers are Terraform's way of abstracting integrations with API control layer of the infrastructure vendors
- By default, Terraform looks for providers in the Terraform Providers Registry - https://registry.terraform.io/browse/providers
- Providers are Terraform's plugins and have a separate release cadence from Terraform itself, with each privder having its own versioning and release cadence
- You can write your own custom providers (out of scope for Terraform Associate certification)
- Terraform locates and installs providers when initializing working directory when **terraform init** command is executed
- Best practice - peg down provider to a specific version to prevent provider side changes from breaking your TF code

```H
## Providers example
provider "azurerm" {
   version = "2.20.0"
   features
}

provider "aws" {
   version = "3.7.0"
   region = "us-east-1"
}
```

Setting debug level logging for TF

```Bash
export TF_LOG=TRACE
terraform init
# Providers get downloaded into hidden folder named .terraform
ls -a
```

### Terraform State Concept

Why state is required?

- Resource tracking - a way for Terraform to keep tabs on what has been deployed
- Critical for TF functionality - acts as a basis for TF to decide whether resource has to be created, modified or destroyed
- terraform.tfstate - flat JSON file which contains metadata about TF deployment and deployed resources
- Stored either locally in the same directory as TF code or remotely
- Allows TF to calculate deployment deltas and create new deployment plans
- Keep state file safe (may contain sensitive data) and backed up

### Terraform Variables and Outputs

#### Variables in Terraform

```Bash
### Variable definition example
variable "var1" {
   description = "Test variable"
   type        = string
   default     = "Test"
}
# variable - reserved keyword
# var1 - user defined variable name
# {} - variable config arguments - description, type, default value

### Variable reference example
var.var1

### Variable validation example
variable "var1" {
   description = "Test variable"
   type        = string
   default     = "Test"
   validation {
      condition     = length(var.var1) > 4
      error_message = "The string must be more than 4 characters"
   }
}

### Sensitive parameter can be used to prevent variable value to show up during TF execution
variable "var1" {
   description = "Test variable"
   type        = string
   default     = "Test"
   sensitive   = true
}

### Variable types
# Base types - string, number, bool

# string - ""
variable "my_id" {
   type  = string
   default = "ID001"
}

# simple list - []
variable "az_names" {
   type    = list(string)
   default = ["us-west-1a"]
}

# complex list - []
variable "docker_ports" {
   type    = list(object({
      internal = number
      external = number
      protocol = string
   }))
   default = [
     {
      internal = 8300
      external = 8300
      protocol = "tcp"
     }
   ]
}

# Complex types - list, set, map, object, tuple
```

- Best practice - keep variables in a separate file **terraform.tfvars**
- Validation block can be added to ensure that only correct value can be provided/define accepted values (TF 0.13+ feature, used to be experimental feature in 0.12)
- Sensitive parameter can be used to prevent variable value to show up during TF execution
- Base types
   - string
   - number
   - bool
- Complex types
   - list
   - set
   - map
   - object
   - tuple
- Runtime variable precedence (from high to low)
   - OS env variables
   - terraform.tfvars

#### Terraform Outputs

Output value are return values that you want to track after a successful Terraform deployment.

```Bash
# Output example
output "instance_ip" {
   description = "VM's private IP"
   value       = aws_instance.my-vm.private_ip
}
# output - reserved keyword
# instance_ip - user provided name
# {} - description & value, value can be assigned directly or using variables references

# Output variable values are shown in the shell when running terraform apply
```

### Terraform Provisioners

- Terraform's way of bootstrapping custom scripts, command or actions
- Can be run either locally (on Terraform host) or remotely (on resources being created by Terraform deployment)
- Within a Terraform code, each individual resource can have its own provisioner defining the connection method (SSH or WinRM) and the actuins/commands to execute
- 2 types
   - Creation-time
   - Destroy-time

#### Terraform Provisioners - Best Practices & Caveats

- HashiCorp recommends using provisioners as a last resort and try using inherent mechanisms within your infra deployment to carry out custom tasks where possible (e.g. it is better to use AWS EC2 instance user data and other resource specific built-in options)
- TF cannot track changes to provisioners as they can take any independent action, hence they are not tracked by TF state files
- Provisioners are recommended for use when you need to invoke actions not covered by Terraform's declarative model or resource inherent options
- If a command within a provisioner returns non-zero return code, it's considered failed and underlying resource is tainted (marks resource to be provisioned again on the next run)

```Bash
### Provisioner example
# provisioner gets run against resource
# null_resource emulates resource life-cycle without any backend actions by default, can be created, destroyed, modified
# create & destroy provisioners

resource "null_resource" "test_resource" {
   provisioner "local-exec" {
      command = "echo 'CREATED' > status.txt"
   }

   provisioner "local-exec" {
      when = destroy
      command = "echo 'DESTROYED' > status.txt"
   }
}

# within provisioners use of TF variable references is not allowed as it can create cyclical dependencies (referencing resource not yet being created)
# so instead of var.ec2vm.id you will ne using self.id within provisioner block to reference resource to wich provisioner is attached

resource "aws_instance" "ec2vm" {
   ami                         = ami-12345
   instance_type               = t2.micro
   key_name                    = aws_key_pair.master-key.key_name
   associate_public_ip_address = true
   vpc_security_group_ids      = [aws_security_group.jenkins-sg.id]
   subnet_id                   = aws_subnet.subnet.id
   provisioner "local-exec" {
      command = "aws ec2 wait instance-status-ok --region us-east-1 --instance-ids ${self.id}"
   }
}
```

### Installing Terraform and Working with Terraform Providers

- Download and Manually Install the Terraform Binary
- Clone over Code for Terraform Providers
- Deploy the Code with Terraform Apply 

```Bash
# Download & manually install TF
# https://developer.hashicorp.com/terraform/downloads
wget -c https://releases.hashicorp.com/terraform/1.3.6/terraform_1.3.6_linux_amd64.zip
unzip terraform_1.3.6_linux_amd64.zip
sudo mv terraform /usr/sbin/
terraform version

# Manually install providers
mkdir providers
cd providers

# TF file with muliple instances of AWS provider targeting different regions and having aliases
cat <<EOF > main.tf
provider "aws" {
  alias  = "us-east-1"
  region = "us-east-1"
}

provider "aws" {
  alias  = "us-west-2"
  region = "us-west-2"
}

resource "aws_sns_topic" "topic-us-east" {
  provider = aws.us-east-1
  name     = "topic-us-east"
}

resource "aws_sns_topic" "topic-us-west" {
  provider = aws.us-west-2
  name     = "topic-us-west"
}
EOF

export TF_LOG=TRACE # enable trace logging to see how providers get downloaded
terraform init
ls -a # to see .terraform directory into which providers get downloaded
terraform fmt # beautifies and makes your code consistent
export TF_LOG= # disable trace logging
terraform plan
terraform apply # we will see in resource ARN that resources got created within different regions
terraform destroy --auto-approve
```

### Using Terraform Provisioners to Set Up an Apache Web Server on AWS

```Bash
# Create EC2 instance and use remote provisioner to set up Apache web server on it
# connection block configures connectivity for provisioned
cat <<EOF > main.tf
#Create and bootstrap webserver
resource "aws_instance" "webserver" {
  ami                         = data.aws_ssm_parameter.webserver-ami.value
  instance_type               = "t3.micro"
  key_name                    = aws_key_pair.webserver-key.key_name
  associate_public_ip_address = true
  vpc_security_group_ids      = [aws_security_group.sg.id]
  subnet_id                   = aws_subnet.subnet.id
  provisioner "remote-exec" {
    inline = [
      "sudo yum -y install httpd && sudo systemctl start httpd",
      "echo '<h1><center>My Test Website With Help From Terraform Provisioner</center></h1>' > index.html",
      "sudo mv index.html /var/www/html/"
    ]
    connection {
      type        = "ssh"
      user        = "ec2-user"
      private_key = file("~/.ssh/id_rsa")
      host        = self.public_ip
    }
  }
  tags = {
    Name = "webserver"
  }
}
EOF

cat <<EOF > setup.tf
provider "aws" {
  region = "us-east-1"
}

#Create key-pair for logging into EC2 in us-east-1
resource "aws_key_pair" "webserver-key" {
  key_name   = "webserver-key"
  public_key = file("~/.ssh/id_rsa.pub")
}


#Get Linux AMI ID using SSM Parameter endpoint in us-east-1
data "aws_ssm_parameter" "webserver-ami" {
  name = "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2"
}

#Create VPC in us-east-1
resource "aws_vpc" "vpc" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_support   = true
  enable_dns_hostnames = true
  tags = {
    Name = "terraform-vpc"
  }

}

#Create IGW in us-east-1
resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.vpc.id
}

#Get main route table to modify
data "aws_route_table" "main_route_table" {
  filter {
    name   = "association.main"
    values = ["true"]
  }
  filter {
    name   = "vpc-id"
    values = [aws_vpc.vpc.id]
  }
}
#Create route table in us-east-1
resource "aws_default_route_table" "internet_route" {
  default_route_table_id = data.aws_route_table.main_route_table.id
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.igw.id
  }
  tags = {
    Name = "Terraform-RouteTable"
  }
}

#Get all available AZ's in VPC for master region
data "aws_availability_zones" "azs" {
  state = "available"
}

#Create subnet # 1 in us-east-1
resource "aws_subnet" "subnet" {
  availability_zone = element(data.aws_availability_zones.azs.names, 0)
  vpc_id            = aws_vpc.vpc.id
  cidr_block        = "10.0.1.0/24"
}


#Create SG for allowing TCP/80 & TCP/22
resource "aws_security_group" "sg" {
  name        = "sg"
  description = "Allow TCP/80 & TCP/22"
  vpc_id      = aws_vpc.vpc.id
  ingress {
    description = "Allow SSH traffic"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  ingress {
    description = "allow traffic from TCP/80"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

output "Webserver-Public-IP" {
  value = aws_instance.webserver.public_ip
}
EOF

terraform init
terraform plan
terraform apply
curl http://<PUBLIC_IP>
```

### Terraform Fundamentals Recap

**Terraform provisioners** - help bootstrap custom commands onto the resources being deployed via Terraform, they can help execute custom scripts and commands on resources. Best practice is to avoid using them if a built-in mechanism is provided by the resource API itself.

**Terraform providers** - plugins that enable Terraform to interface with the API layer of various cloud platforms and environments, Terraform relies on these plugins to interact with remote systems. Without providers, Terraform can't manage any kind of infrastructure. See [Providers Overview](https://developer.hashicorp.com/terraform/language/providers)

Providers can be sourced in a different ways:
- By default, Terraform looks for providers in the Terraform provider registry - https://registry.terraform.io
- You can reference providers from an internal registry in your Terraform code
- You can reference providers locally in your Terraform configuration

**Terraform variables**

- Can be predefined in a variables.tf or terraform.tfvars file
- They can be included in the command line options - ```terraform apply -var variable_name="value"```
- They can be pulled down from Terraform Cloud and referenced in your code - Terraform Cloud workspaces can set values for input and shell environment variables

```Bash
# Variable definition can contain default value
variable "replicas" {
  type = number
  default = 5
}

# Default value can be redefined when running terraform apply
terraform apply -var replicas=1
```

**Terraform state**

- stored by default in a local file named **terraform.tfstate**, but it can also be stored remotely, which works better in a team environment; more information - [Terraform State Overview](https://developer.hashicorp.com/terraform/language/state)
- JSON format file

**Terraform output variables**

- When using remote state, **root module outputs can be accessed by other configurations** via a **terraform_remote_state data source**
- **A root module can use outputs to print certain values in the CLI output after running terraform apply**
- **A child module can use outputs to expose a subset of its resource attributes to a parent module**

## Terraform State

### Terraform State Command

Terraform State

- Maps real-world resources to Terraform configuration
- By default, stored locally in a terraform.tfstate file, but can be stored in AWS S3 bucket or in Azure blob container
- Prior to any modification operation, Terraform refreshes the state file
- Resource dependency metadata is also tracked via the state file
- Helps to boost deployment performance by caching resource attributes for subsequent use

Terraform State Command

- Terraform state command is an utility for manipulating and reading the Terraform state file
- Usage scenarios:
  - Advanced state management
  - Manually remove a resource from a Terraform state file
  - List out tracked resources and their details (via state and list subcommands)

Common Terraform State Sub-Commands

```Bash
- terraform state list # list out all the resources tracked by the TF state file
- terraform state rm # delete a resource from the TF state file
- terraform state show # show details of a resource tracked in the TF state file

cat <<EOF > main.tf
# Configure Docker provider
terraform {
  required_providers {  
    docker = {
      source = "kreuzwerker/docker"
      version = "2.24.0"
    }
  }
}

provider "docker" {
  host = "unix:///var/run/docker.sock"
}

# Image to be used by container
resource "docker_image" "terraform-centos" {
  name         = "centos:7"
  keep_locally = true
}

# Create a container
resource "docker_container" "centos" {
  image = docker_image.terraform-centos.latest
  name  = "terraform-centos"
  start = true
  command = ["/bin/sleep", "500"]
}
EOF

terraform init
terraform apply
terraform state list
terraform state show docker_container.centos
terraform state show docker_image.terraform-centos
terraform state rm docker_container.centos # remove resource from the state aka "unmanage" make resource not not managed by TF
```

### Local and Remote State Storage

#### Local State Storage

- Default option/behavior
- Saves Terraform state locally on your system
- Used when not working in a team/for testing
- Allows locking state so that parallel executions don't coincide

#### Remote State Storage

- Optional behavior
- Saves state to a remote data storage - AWS S3, Azure Blob Container, Google Storage
- Allows sharing state file between distributed teams - remote state accessible to multiple teams/users for collaboration
- Allows locking state so that parallel executions don't coincide - **not supported for all remote storage backends**
  - Supported by Azure Blob Container, AWS S3, Google Cloud Storage, HashiCorp Consul
- Enables sharing "output" values with other Terraform configuration or code
  - Execute > Save state on remote storage
  - Outputs accessible everywhere

### Persisting Terraform State in AWS S3

Remote state backend configured using special TF block named backend.

```Bash
# Configure profile & pre-create bucket
aws --profile demo configure
aws --profile demo s3api create-bucket --bucket s3bucket03012022

# Define S3 backend for remote state storage
terraform {
  backend "S3" {
    region = "us-east-1"
    key    = "terraformstatefile"
    bucket = "s3bucket03012022"
  }
}

# backend.tf
cat <<EOF > backend.tf
terraform {
  required_providers {  
    docker = {
      source = "kreuzwerker/docker"
      version = "2.24.0"
    }
  }
  required_version = ">= 0.13"
  backend "s3" {
    profile = "demo"
    region  = "us-east-1"
    key     = "terraformstatefile"
    bucket  = "s3bucket03012022"
  }
}
EOF

#  main.tf
cat <<EOF > main.tf
provider "docker" {
  host = "unix:///var/run/docker.sock"
}

resource "docker_image" "nginx" {
  name = "nginx"  
}

resource "docker_container" "nginx" {
  image = docker_image.nginx-image.latest
  name  = "nginx"
  ports {
    internal = 80
    external = var.external_port
    protocol = "tcp"
    }
}

output "url" {
  description = "Browser URL for container site"
  value       = join(":",["http://localhost", tostring(var.external_port)])
}
EOF

# variables.tf
cat <<EOF > varibles.tf
variable "external_port" {
  type    = number
  default = 8080
  validation {
    condition = can(regex("8080|80", var.external_port))
    error_message = "Port values can only be 8080 or 80"
  }
}
EOF

terraform init
terraform plan
terraform apply
terraform apply -var external_port=9999 # test var validation

aws --profile demo s3 cp s3://s3bucket03012022/terraform.tfstate .
less terraform.tfstate

terraform destroy
```

### Exploring Terraform State Functionality

- Check Terraform and Minikube Status
- Clone Terraform Code and Switch to the Proper Directory
- Deploy Terraform Code And Observe the State File

```Bash
# https://github.com/linuxacademy/content-hashicorp-certified-terraform-associate-foundations.git
terraform version
minikube status

# main.tf
cat <<EOF > main.tf
provider "kubernetes" {
  config_path = "~/.kube/config"
}

resource "kubernetes_deployment" "tf-k8s-deployment" {
  metadata {
    name = "tf-k8s-deploy"
    labels = {
      name = "terraform-k8s-deployment"
    }
  }

  spec {
    replicas = 2

    selector {
      match_labels = {
        name = "terraform-k8s-deployment"
      }
    }

    template {
      metadata {
        labels = {
          name = "terraform-k8s-deployment"
        }
      }

      spec {
        container {
          image = "nginx"
          name  = "nginx"

        }
      }
    }
  }
}
EOF

# service.tf - creates K8s service
cat <<EOF > service.tf
resource "kubernetes_service" "tf-k8s-service" {
  metadata {
    name = "terraform-k8s-service"
    labels = {
      name = "tf-k8s-deploy"
    }
  }
  spec {
    port {
      port        = 80
      target_port = 80
      node_port   = 30080
    }
    type = "NodePort"
  }
}
EOF

terraform init
terraform plan
ls # no statefile generated yet
terraform apply
ls # terraform.tfstate file created
kubectl get pods
terraform state list
terraform state show kubernetes_deployment.tf-k8s-deployment
# check N of replicas for deployment tracked by state file
terraform state show kubernetes_deployment.tf-k8s-deployment | egrep replicas
# change N of replicas in TF code
vi main.tf # change replicas = 2 to replicas = 4 and save
terraform plan
terraform apply
kubectl get pods
# check N of replicas for deployment tracked by state file
terraform state show kubernetes_deployment.tf-k8s-deployment | egrep replicas

terraform destroy
kubectl get pods # pods terminated and removed
ls # terraform.tfstate is cleaned out, terraform.tfstate.backup file appeared
less terraform.tfstate.backup # last good working backup
```

### Recap Terraform State

- terraform.tfstate - local file which stores Terraform state, after deploying your infrastructure, Terraform generates this state file locally within your Terraform configuration directory
- remote state can be configured in the terraform block, using the backend attribute, there are a number of preset platforms on which you can set Terraform to store state remotely, including AWS S3 storage and Google Cloud storage

```H
terraform {
  backend "s3" {
    profile = "demo"
    region  = "us-east-1"
    key     = "terraformstatefile"
    bucket  = "s3bucket03012022"
  }
}
```
- remote state provides granular access, integrity, security, availability, and collaboration
- Terraform handles dependencies in your infra when deploying or destroying resources using state file, the Terraform state file is a map of all resources and their dependencies. It is essential for Terraform to work properly
- Terraform state maps real-world resources to Terraform configuration/code, it is a map between real-world resource IDs and configurations to logical resources in your Terraform code

```Bash
terraform state list # list all of the resources in the Terraform state

```

## Terraform Modules

### Accessing and Using Terraform Modules

Terraform Modules

- A module is a container for multiple resources that are used together
- Every TF Configuration has at least one module, called the **root module**, which consists of code files in your main working directory

Accessing Terraform Modules

- Modules can be downloaded or referenced from
  - Terraform Public Registry (modules get downloaded and placed into hidden directory)
  - Private Registry
  - Local system - sourced via local path
- Modules are referenced using a **module block**
- As a best practic specific version of module should be always set as required, to avoid module updates causing unwanted effects on your deployments
- Modules can optionally take input and provide outputs to plug back into your main code

```Bash
module "my-vpc-module" {
  source = "./modules/vpc"
  version = "0.0.5" 
  region = var.region
}
# module - reserved keyword
# my-vpc-module - module name
# {} - parameters
# source - module source, mandatory
# version - module version, as a best practic specific version of module should be always set as required, to avoid module updates causing unwanted effects on your deployments
# region - input parameter for module, module can have multiple input parameters
# other allowed parameters: count, for_each, providers, depends_on
# count - allows spawning of multiple separate instances of the module's resources
# for_each - allows iterating over complex variables
# providers - allows to tie specific providers to the module
# depends_on - allows to set module dependencies
```

Consuming outputs provided by a child module

```Bash
# Accessing module outputs in code
resource "aws_instance" "my-vpc-module" {
  .... # other arguments
  subnet_id = module.my-vpc-module.subnet_id
}
# module.my-vpc-module.subnet_id - module output - module.module_name.output_name
```

### Interacting with Terraform Module Inputs and Outputs

#### Module Inputs

- Module inputs are arbitrarily named parameters that you pass inside the module block
- This modules can be used as variables inside the module code
- To reference input within the module we use standard TF variable notation

```Bash
module "my-vpc-module" {
  source      = "./modules/vpc" # URL or path
  server-name = 'us-east-1' # input parameter(s) for module
}
# to reference input within the module we use standard TF variable notation
var.server-name
```

#### Module Outputs


>>>

## 7 Implement and Maintain State

State file VS State

State of the State

Terraform stores state in JSON formatted file called terraform.tfstate, sample content:

```JSON

```

Avoid direct man edits of this file. You can open it directly as a file and review

By default Terraform uses local state storage (terraform.tfstate file stored locally), remote storage can be setup if you working in a team through use of the Terraform backends feature.

Recommended way of state inspection is with **terraform list** command

- list - list resources in the state
- mv - move an item in the state
- pull - pull current state and output to stdout
- push - update remote state from a local state file
- rm - remove instances from the state
- show - show a resource in the state (-json to get json blob output)

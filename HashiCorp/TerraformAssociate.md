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

### Understanding IaC Recap

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

### IaC With Terraform Recap

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

### Terraform State Recap

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

- Module outputs declared inside TF module code can be feed back into the root module or your main code
- Output invocation convention in TF code - module.<name-of-module>.<name-of-output>
- 

```Bash
# output defined in TF module
output "ip_address" {
  value = aws_instance.private_ip
}
# output can be called from main TF code
# output invocation convention in TF code - module.<name-of-module>.<name-of-output>
module.my-vpc-module.ip_address

```

### Building and Testing a Basic Terraform Module

```Bash
# verify TF install and create TF project folders structure
terraform version
mkdir terraform_project
cd terraform_project
mkdir -p modules/vpc
cd modules/vpc/

# main.tf
cat <<EOF > main.tf
provider "aws" {
  region = var.region
}

resource "aws_vpc" "this" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "this" {
  vpc_id     = aws_vpc.this.id
  cidr_block = "10.0.1.0/24"
}

data "aws_ssm_parameter" "this" {
  name = "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2"
}
EOF

# variables.tf
cat <<EOF > variables.tf
variable "region" {
  type    = string
  default = "us-east-1"
}
EOF

# outputs.tf - module dir
cat <<EOF > outputs.tf
output "subnet_id" {
  value = aws_subnet.this.id
}

output "ami_id" {
  value = data.aws_ssm_parameter.this.value
}
EOF

cd ~/terraform_project/

# main.tf
cat <<EOF > main.tf
variable "main_region" {
  type    = string
  default = "us-east-1"
}

provider "aws" {
  region = var.main_region
}

module "vpc" {
  source = "./modules/vpc"
  region = var.main_region
}

resource "aws_instance" "my-instance" {
  ami           = module.vpc.ami_id
  subnet_id     = module.vpc.subnet_id
  instance_type = "t2.micro"
}
EOF

# outputs.tf - main project dir
cat <<EOF > outputs.tf
output "PrivateIP" {
  description = "Private IP of EC2 instance"
  value       = aws_instance.my-instance.private_ip
}
EOF

terraform fmt -recursive # fmt = Reformat your configuration in the standard style
terraform init # fetches providers & modules
terraform validate # validates code, validate = Check whether the configuration is valid
terraform plan
terraform apply --auto-approve
terraform state list
terraform destroy
```

### Terrafrom Modules Recap

- TF modules are used to make code reusable elsewhere and avoid reinventing the wheel; modules are the main way to package and reuse resource configurations with Terraform. More info: [Terraform Modules Overview](https://developer.hashicorp.com/terraform/language/modules)
- TF modules sourcing
  - Terraform Registry - you can reference modules that are hosted on the Terraform public registry, which contains a collection of all publicly available modules
  - Private Registry - you can also reference modules in a private registry that is hosted by yourself or your organization
  - Local System - you can have the module code saved in a local folder on your system and reference that folder using its path
- TF output invocation convention when using module output inside your main code is module.<name-of-module>.<name-of-output>
- To use TF outputs in main TF code we use **output block in the TF module code**

```Bash
# TF outputs defined in output block of TF module, and referenced in main TF code using module.<name-of-module>.<name-of-output> notation

# TF prod-module output definition
output "returned-variable" {
  value = "1"
}
# Calling output from main TF code
module.prod-module.returned-variable
```

- Using input in TF module - input variable is being passed using the standard TF variable reference notation, var.

```Bash
module "my-test-module" {
  source = "./testm"
  version = "0.0.5"
  region = var.datacenter # TF input
}
# input variable is being passed using the standard variable reference notation, var.
```

## Built-in Functions and Dynamic Blocks

### Terraform Built-in Functions

- TF comes pre-packaged with functions to help you transform and combine values
- User-defined functions are not allowed - only built-in ones can be used
- General syntax is similar to other program languages: function_name(arg1, arg2, ...)
- Buil-in functions make TF code dynamic and flexible
- Complete list of functions can be found [here](https://developer.hashicorp.com/terraform/language/functions), some examples
  - file - insert file
  - max - determine max integer value from a provided list
  - flatten - create single list out of provided set of lists

```Bash
variable "project_name" {
  type  = string
  value = "prod"
}

resource "aws_vpc" "my-vpc" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = join("-",["terraform", var.project-name]) # resulting value: terraform-prod
  }
}
```

Checking TF built-in functions

- TF console provides CLI for interactive testing/evaluating expressions

```Bash
terraform console
max(3,5,9,11) # get numeric max from the list
timestamp() # get UTC time stamp
join(" ",["Hello", "world"]) # join list elements using provided separator
contains(["apple", "banana", "orange", 1, 2, 3], "banana") # check if list contains specific element, returns true/false
```

### Terraform Type Constraints (Collections & Structural)

Type Constraints - TF Variables

- Type constraints control the type of variable values
  - Primitive - single type value (number, string, bool)
  - Complex - multiple types in a single variable (list, tuple, map, object)

Complex Types - Collections

- Collection types allow multiple values of one primitive type to be grouped together
- Constructors of these collections include
  - list(type)
  - map(type)
  - set(type)

```Bash
# Complex Types - Collection Example
variable "training" {
  type = list(string)
  defult = ["US", "ES"]
}
```

Complex Types - Structural

- Structural types allow multiple values of different primitive types to be grouped together
- Constructors of these collections include
  - object(type)
  - tuple(type)
  - set(type)

```Bash
# Complex Types - Structural Example
variable "instructor" {
  type = object({ # object type can contain several variables of primitive types within it
    name = string # var of primitive type string
    age  = number # var of primitive type number
  })
}
```
Dynamic Types - The "any" constraint

- any is a placeholder for a primitive type yet to be decided
- Actual type will be determined at runtime

```Bash
# Dynamic Type - any example
variable "data" {
  type = list(any)
  default = [1, 42, 7]
}
# TF will recognize passed in value types at runtime
```

### Terraform Dynamic Blocks

What are Dynamic Blocks?

- Dynamically constructs repeatable configuration blocks inside TF resources
- Supported within the following block types
  - resource
  - data
  - provider
  - provisioner
- Used to make your code look cleaner - removing need to repeatitive chunks of nested block resources
- Dynamic blocks expect a complex variable type to iterate over
- Acts like a for loop which outputs a nested block for each element in your variable
- Be careful not to overuse dynamic blocks in main code as it makes your code difficult to read and maintain
- Best practice is to use dynamic blocks when you need to hide details to build a cleaner UI when wrtiting reusable modules
  
```Bash
# TF code w/o dynamic block
resource "aws_security_group" "my-sg" {
  name   = 'my-aws-security-group'
  vpc_id = aws_vpc.my-vpc.id
  ingress {
    from_port = 22
    to_port   = 22
    protocol  = "tcp"
    cidr_blocks = ["1.2.3.4/32"]
  }
  ingress {
    ... # more ingress rules
  }
}

# TF code with dynamic block
resource "aws_security_group" "my-sg" {
  name   = 'my-aws-security-group'
  vpc_id = aws_vpc.my-vpc.id
  dynamic "ingress" { # dynamic block
    for_each = var.rules { # complex var to iterate over
      from_port   = ingress.value["port"] # nested content block defines the body of each generated block, using the variable you provided
      to_port     = ingress.value["port"]
      protocol    = ingress.value["proto"]
      cidr_blocks = ingress.value["cidrs"]
    }
  }
}

# TF code to pass in complex var to dynamic block
variable "rules" {
  default = [
    { # 1st item within the complex var type
      port        = 80
      proto       = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    },
    { # 2nd item within the complex var type
      port   = 22
      proto = "tcp"
      cidr_blocks = ["1.2.3.4/32"]
    }
  ]
}

# TF dynamic block code which uses var set above
resource "aws_security_group" "my-sg" {
  name   = 'my-aws-security-group'
  vpc_id = aws_vpc.my-vpc.id
  dynamic "ingress" { # dynamic block
    for_each = var.rules { # complex var to iterate over
      from_port   = ingress.value["port"] # nested content block defines the body of each generated block, using the variable you provided
      to_port     = ingress.value["port"]
      protocol    = ingress.value["proto"]
      cidr_blocks = ingress.value["cidrs"]
    }
  }
}
```

### Using Terraform Dynamic Blocks and Built-in Functions to Deploy to AWS

```Bash
# main.tf
cat <<EOF > main.tf
provider "aws" {
  region = "us-east-1"
}

data "aws_ssm_parameter" "ami_id" {
  name = "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2"
}


module "vpc" {
  source = "terraform-aws-modules/vpc/aws"

  name = "my-vpc"
  cidr = "10.0.0.0/16"

  azs            = ["us-east-1a"]
  public_subnets = ["10.0.1.0/24"]


}


resource "aws_security_group" "my-sg" {
  vpc_id = module.vpc.vpc_id
  name   = join("_", ["sg", module.vpc.vpc_id])
  dynamic "ingress" {
    for_each = var.rules
    content {
      from_port   = ingress.value["port"]
      to_port     = ingress.value["port"]
      protocol    = ingress.value["proto"]
      cidr_blocks = ingress.value["cidr_blocks"]
    }
  }
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "Terraform-Dynamic-SG"
  }
}

resource "aws_instance" "my-instance" {
  ami             = data.aws_ssm_parameter.ami_id.value
  subnet_id       = module.vpc.public_subnets[0]
  instance_type   = "t3.micro"
  security_groups = [aws_security_group.my-sg.id]
  user_data       = fileexists("script.sh") ? file("script.sh") : null # if file exists we pass in its contents as input to user_data parameter, otherwise it will be set to null
}
EOF

# variables.tf - list of parameters for 3 different rules/ingress blocks - Security Group Inbound Rules
cat <<EOF > variables.tf
variable "rules" {
  type = list(object({
    port        = number
    proto       = string
    cidr_blocks = list(string)
  }))
  default = [
    {
      port        = 80
      proto       = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    },
    {
      port        = 22
      proto       = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    },
    {
      port        = 3689
      proto       = "tcp"
      cidr_blocks = ["6.7.8.9/32"]
    }
  ]


}
EOF

# script.sh
cat <<EOF > script.sh
#!/bin/bash
sudo yum -y install httpd
sudo systemctl start httpd && sudo systemctl enable httpd
EOF

# outputs.tf
CAT <<EOF > outputs.tf
output "Web-Server-URL" {
  description = "Web-Server-URL"
  value       = join("", ["http://", aws_instance.my-instance.public_ip])
}

output "Time-Date" {
  description = "Date/Time of Execution"
  value       = timestamp()
}

EOF

terraform version
terraform fmt
terraform init
terraform validate
terraform plan
terraform apply --auto-approve
terraform destroy --auto-approve
```

### Built-in Functions and Dynamic Blocks Recap

- Structural variable type allows multiple values of various primitive types to be grouped together as a single value

```Bash
# Structural variable type allows multiple values of various primitive types to be grouped together as a single value
# Below the variable training has 2 separate types of values within it namely, a string and a number

variable "training" { # structural var with 2 different tupes of values - string & number
  type = object({
    name = string
    age = number
  })
  default = {
    name = "Ryan"
    age = 36
  }
}
```

- Dynamic blocks supported within the following block types
  - resource
  - data
  - provider
  - provisioner
- Dynamic blocks CANNOT be used with lifecycle blocks, as Terraform must process this type of block before it is safe to evaluate expressions; more information - [Dynamic Blocks](https://developer.hashicorp.com/terraform/language/expressions/dynamic-blocks)

- Built-in functions
  - Terraform comes pre-packaged with a number of built-in functions; users cannot create their own functions like in a programming language
  - The join function concatenates two strings using specifed delimiter

- A primitive type includes basic data types that aren't comprised of more than one value or of nested data values, such as a numeric value, a sequence of characters, or binary (true/false) logic.
  - Number
  - String
  - Boolean

- Collection variable types allow multiple values of one primitive type variable to be grouped together; a collection type allows multiple values of one other type to be grouped together as a single value; more information - [Terraform Collection Types](https://developer.hashicorp.com/terraform/language/expressions/type-constraints#collection-types)

## Terraform CLI

### Terraform fmt, taint and import

#### Terraform fmt (format)

- Formats TF code for readability
- Helps in keeping code consistent
- Safe to run at any time
- Recommended use cases
  - Before pushing your code to version control
  - After upgrading TF or its modules
  - Any time you've made changes to code

```Bash
terraform fmt # looks for any .tf files and format them, outputs formatted files list
```

#### Terraform taint

- Taints a resource, forcing it to be destroyed and recreated
- Modifies the state file, which triggers the recreation workflow
- Tainting a resource may cause other resources to be modified
- Recommended usecases
  - To cause provisioners to run
  - Replace misbehaving resources forcefully
  - To mimic side effects of recreation not modelled by any attributes of the resource

```Bash
terraform taint RESOURCE_ADDRESS
```

#### Terraform import

- Maps existing resources to Terraform using an "ID"
- "ID" is dependent on the underlying vendor, for example to import an AWS EC2 instance you'll need to provide its instance ID
- Importing the same resource to multipl TF resources can caue unpredictable behavior and is not recommended
- Recommended usecases
  - When you need to work with existing resources
  - When you're not allowed to create new resources
  - When you're not in control of creation process of infrastructure

```Bash
terraform import RESOURCE_ADDRESS ID
```

#### Terraform Configuration Block

- A special configuration block for controlling TF behavior
- This block only allows constant values, named resources and variables are not allowed
- Examples include
  - Configuring backend for storing state files
  - Specifying a required TF version
  - Specifying a required TF provider version and its requirements
  - Enable and test TF experimental features
  - Passing metadata to providers

```Bash
# TF configuration  block example
terraform {
  required_version = ">=0.13.0"
  required_providers {
    aws = ">=3.0.0"
  }
}
```

### Terraform Workspaces

- The TF workspaces are alternate state files within the same working directory
- TF starts with a single workspace which is always called **default**, it cannot be deleted
- Recommended usecases
  - Test changes using parallel, distinct copy of infrastructure - one wokrspace target DEV env, other PROD
  - It can be modelled against branches in version control such as Git
- Workspaces are meant to share resources and to help enable collaboration
- Access to a workspace name is provided through the ${terraform.workspace} variable

```Bash
terraform workspace new <WORKSPACE_NAME> # create a workspace
terraform workspace select <WORKSPACE_NAME> # select a workspace
```

- TF workspace variable usage example

```Bash
# TF workspace variable usage example 1
resource "aws_instance" "example" {
  count terraform.workspace == "default" ? 5 : 1 # if workspace var = default create 5 instances, if not 1
  # ... other arguments
}

# TF workspace variable usage example 2
resource "aws_s3_bucket" "bucket" {
  bucket = "asdfgbucket-${terraform.workspace}" # create workspace specific bucket adding workspace name to bucket
  acl    = "private"
}
```

```Bash
terraform workspace list
terraform workspace new test
terraform workspace list
terraform workspace select default
```

### Debugging Terraform

#### TF_LOG and TF_LOG_PATH

- TF_LOG is an environment variable for enabling verbose logging in TF; by default, it will send logs to stderr (standard error output)
- Can be set to the following levels: TRACE, DEBUG, INFO, WARN, ERROR
- TRACE is the most verbose level of logging and the most reliable one
- To persist logged output, set the TF_LOG_PATH environment variable
- Setting logging environment for Terraform on Linux

```
export TF_LOG=TRACE
export TF_LOG_PATH=./terraform.log
```

### Terraform CLI Commands (fmt, taint, and import)

```Bash
# main.tf
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
                "echo '<h1><center>My Website via Terraform Version 1</center></h1>' > index.html",
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

# setup.tf
cat <<EOF > main.tf
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
[cloud_user@ip-10-0-1-29 section4-hol1]$
[cloud_user@ip-10-0-1-29 section4-hol1]$ cat setup.tf
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

# Use the 'fmt' Command to Format Any Unformatted Code Before Deployment
terraform fmt # outputs list of formatted files
terraform plan
terraform apply
curl http://<WEB_SERVER_PUBLIC_IP>

# Use the 'taint' Command to Replace a Resource
# taint marks resource for deletion so that will be deleted on next TF apply run
# TF do not track provisioners, so changing TF provisioner code won't trigger its config on the next apply - hence tain can be used to trigger that
# Edit provisioner definition in main.tf and then taint it
terraform taint aws_instance.webserver
# we can see that resource gets tainted by looking into terraform.tfstate file
# "status": "tainted"
terraform apply
curl http://<WEB_SERVER_PUBLIC_IP>

# Use the 'import' Command to Import a Resource
# Add resource into main.tf to create a placeholder for resource being imported
cat <<EOF >> main.tf

resource "aws_instance" "webserver2" {}
EOF

# Impot resource into the state
terraform import RESOURCE_NAME RESOURCE_ID
terraform import aws_instance.webserver2 i-0f64cb7605e2be076

# Modify the Imported Resource
# Append aws_instance.webserver2 resource with params
#  ami                         = data.aws_ssm_parameter.webserver-ami.value
#  instance_type               = "t3.micro"
#  subnet_id                   = aws_subnet.subnet.id

terraform fmt
terraform apply
```

### Using Terraform CLI Commands (workspace and state) to Manipulate a Terraform Deployment

```Bash
# main.tf

cat <<EOF > main.tf
provider "aws" {
  region = terraform.workspace == "default" ? "us-east-1" : "us-west-2"
}

#Get Linux AMI ID using SSM Parameter endpoint in us-east-1
data "aws_ssm_parameter" "linuxAmi" {
  name = "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2"
}

#Create and bootstrap EC2 in us-east-1
resource "aws_instance" "ec2-vm" {
  ami                         = data.aws_ssm_parameter.linuxAmi.value
  instance_type               = "t3.micro"
  associate_public_ip_address = true
  vpc_security_group_ids      = [aws_security_group.sg.id]
  subnet_id                   = aws_subnet.subnet.id
  tags = {
    Name = "${terraform.workspace}-ec2"
  }
}

EOF

# network.tf
cat <<EOF > network.tf
#Create VPC in us-east-1
resource "aws_vpc" "vpc_master" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "${terraform.workspace}-vpc"
  }

}

#Get all available AZ's in VPC for master region
data "aws_availability_zones" "azs" {
  state = "available"
}

#Create subnet # 1 in us-east-1
resource "aws_subnet" "subnet" {
  availability_zone = element(data.aws_availability_zones.azs.names, 0)
  vpc_id            = aws_vpc.vpc_master.id
  cidr_block        = "10.0.1.0/24"

  tags = {
    Name = "${terraform.workspace}-subnet"
  }
}


#Create SG for allowing TCP/22 from anywhere, THIS IS FOR TESTING ONLY
resource "aws_security_group" "sg" {
  name        = "${terraform.workspace}-sg"
  description = "Allow TCP/22"
  vpc_id      = aws_vpc.vpc_master.id
  ingress {
    description = "Allow 22 from our public IP"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  tags = {
    Name = "${terraform.workspace}-securitygroup"
  }
}
EOF

# Create a new workspace, deploy into it, then deploy into default
terraform workspace list
terraform workspace new test
terraform workspace list # test is set as current/default
terraform init
terraform state list
terraform workspace select default
terraform state list

# Workspaces folder structure
ls
# it will show terraform.tfstate & terraform.tfstate.d
ls terraform.tfstate.d/ # this folder will contain workspace subfolder(s) containing workspace specific terraform state file (terraform.tfstate)


# Clean up
terraform workspace select test
terraform destroy --auto-approve
terraform workspace select default
terraform workspace delete test
terraform destroy --auto-approve
```

### Terraform CLI Recap

- TF_LOG=TRACE - maximum logging level for TF; You can set TF_LOG to the TRACE, DEBUG, INFO, WARN, or ERROR log level to change the verbosity of the log. TRACE is the default log level, and it is the most verbose.
- terraform taint - manually marks a Terraform-managed resource as tainted, forcing it to be destroyed and re-created on the next apply. More info: [Terraform Taint Command](https://developer.hashicorp.com/terraform/cli/commands/taint)
- When working locally, Terraform always starts off with a single workspace called **default** that cannot be deleted. A Terraform configuration always has a default workspace associated with it. This default workspace always exists regardless of whether or not you create new workspaces locally and it cannot be deleted.

```Bash
terraform fmt # formats your Terraform code for readability and consistency; terraform fmt command can be safely run at any time, but is especially useful before pushing code to a version control system, as it makes your code look cleaner and more consistent

terraform workspace select <WORKSPACE_NAME> # selects or switches to an existing workspace of your choice, using the name you have provided
terraform import # brings external, unmanaged resources into your Terraform configuration to be tracked and managed by it; takes an existing resource that is not managed by Terraform and maps it to a resource within Terraform code using an ID
terraform taint <RESOURCE_TYPE.RESOURCE_NAME> # marks the resource as tainted in the state file and it will be deleted and re-created upon the next terraform apply
```

## Terraform Cloud and Enterprise Offerings

- Best practices to secure Terraform code and deployments
  - HashiCorp Sentinel
  - Terraform Vault Provider
- Difference between Terraform OSS and Enterpise offerings

### Benefits of Sentinel (Embedded Policy-as-Code Framework)

HashiCorp Sentinel - Policy as Code

- Enforces policies on your code
- Has its own policy language - **Sentinel language**
- Designed to be approachable by non-programmers
- Within Terraform Enterprise Sentinel integration runs after terraform plan and before terraform apply
- Policies have access to the data in the created plan, the state of the resources at the time of the plan, and the configuration at the time of the plan

HashiCorp Sentinel - Benifits

- Sandboxing - guardrails for automation to prevent accidental deployments (e.g. stop dev user to deploy into prod workspace)
- Codification - easier understanding, better collaboration
- Version control
- Testing and automation

HashiCorp Sentinel - Usecases

- For enforcing CIS (Center for Internet Security) security standards across AWS accounts
- Checking to make sure only specific type of EC2 instances can be created by TF code, e.g. t3.micro
- Ensure that no AWS security groups allow traffic from all IP addresses on port 22

Sentinel Sample Code for Terraform

```Bash
# Ensure that all EC2 instances have at least 1 tag
import "tfplan"

main = rule {
  all tfplan.resources.aws_instance as _, instances {
    all instances as _ r {
      (length(r.applied.tags) else 0) > 0
    }
  }
}
```

### Best Practice: Terraform Vault Provider for Injecting Secrets Securely

What is HashiCorp Vault?

- Secret Management Software
- **Dynamically provisions credentials and rotates them**
- **Encrypts data in transit and rest** and provides fine-grained access to secrets using ACLs; secrets examples: usernames and passwords, DB credentials, API tokens, TLS certificates
- Helps preventing credentials sprawl - credentials enging up in plain text files, DBs, config files and so on which creates security risk/increases attack surface

Terraform Vault Provider

Injectig Vault Provider Secrets into TF Workflow:

1) Vault admin stores long-lived credentials in Vault, configures permissions for temporary credentials when generated
2) Terraform operator uses vault provider within TF code and runs terraform apply command
3) TF reaches out to Vault for temporary credentials
4) Vault returns temporary, short lived credentials
5) TF deploys using the credentials returned by Vault and, after a configurable amount of time, those temp credentials are deleted by vault

HashiCorp Vault Benefits

- Developers don't need to manage long-lived credentials
- Inject secrets into your TF deployment at runtime
- Fine-grained ACLs for access to temporary credentials

### Benefits of Terraform Registry and Terraform Cloud Workspaces

#### Terraform Registry

- A repository of publicly available Terraform providers and modules - first/by default look up location for providers and modules
- 3rd parties can publsh and share modules too
- https://registry.terraform.io
- Possibility to collaborate with other contributors to make changes to **providers** and **modules**
- Providers tiers: ifficial, verified, community
- Can be directly referenced in TF code

#### Terraform Cloud Workspaces

- Workspaces directories hosted in TF Cloud
- Stores old versions of state files by default
- Maintains a record of all execution activity
- All TF commands executed on managed TF Cloud VMs
- Deployments can be triggered via TF workspaces API, version control systm triggers (GitHub actions), TF Cloud UI

### Differentiating Between Terraform OSS and Terraform Cloud Workspaces

Terraform OSS Workspaces

- Stores alternate state files in the same working directory

Terraform OSS Workspaces vs. Terraform Cloud Workspaces - compare and contrast

| Component               | Workspace                                                   | Cloud Workspace                                                           |
|-------------------------|-------------------------------------------------------------|---------------------------------------------------------------------------|
| Terraform Configuration | On disk                                                     | In linked version control repository or periodically uploaded via API/CLI |
| Variable Values         | As .tfvars files, as CLI arguments, or in shell environment | In wokrspace in TF cloud                                                  |
| State                   | On disk or in remote backend                                | In workspace in TF cloud                                                  |
| Credentials and Secrets | In shell environment or entered at prompts                  | In workspace in TF cloud, stored as sensitive variables                   |

### Benefirs of Terraform Cloud - Summary

- Collaboration oriented TF workflow
  - Remote TF execution
  - Workspace based org model
  - Version control integration (GitHub, Bitbucket etc.)
  - Remote State management and CLI integration
  - Private TF module registry
  - Cost estimation and Sentinel features (policies enforcement)

### Terraform Cloud and Enterprise Summary

Security Best Practices

- Best practices for securing secrets and deployments
  - Sentinel - enforce secure deployments via policies
  - Vault Provider - store and inject secrets securely during Terraform deployments

Workspaces

- Terraform OSS workspaces VS Terraform Cloud workspaces
  - Terraform OSS: workspaces generate and track different state files against the same TF code in a directory, hosted locally on your systems
  - Terraform Cloud: workspaces are hosted in the cloud and track configuration, variables, state, and secrets and can be interacted with programmatically

Terraform Cloud

- General features of TF Cloud and Enterprise offerings
  - Collaboration (API-based workflows, version control, trigger-based deployments)
  - Sentinel integration
  - Cost estimation, shared workspaces, access-based controls

### Terraform Cloud and Enterprise Recap

- Terraform public registry - a repository of publicly available Terraform providers and modules; the Terraform public registry links end users with the providers that power all of Terraform’s resource types and/or helps them find modules for quickly deploying common infrastructure configurations. More information: [Terraform Registry](https://registry.terraform.io/)
- HashiCorp Sentinel - is a policy-as-code framework that enforces adherence to policies within your Terraform code which helps make deployments more secure and act as protection against accidental deployments; the policies inherent to the Sentinel framework ensure that dangerous or malicious Terraform code is blocked before it gets executed or applied via the terraform apply command.
- HashiCorp Vault - a centralized secrets management software which can store your long-lived credentials in a secure way and dynamically inject short-lived, temporary keys to Terraform at deployment.
- Tertaform registry hosts
  - Publicly available Terraform providers
  - Publicly available modules
- Terraform Vault provider
  - Provides short-lived, temporary credentials for users with only the permissions needed for infrastructure creation
  - A secure place to manage access to the secrets for your Terraform configurations, in addition to integrating with other popular cloud vendors
  - Allows you to store sensitive data securely that can be used for your Terraform configurations
- Vault allows you to provide short-lived, temp credentials that allow for users to have only the permissions they need to deploy the infrastructure. The credentials will rotate according to the rotation schedule you define. This allows you to not have to worry about keeping up with long-lived credentials.
- Vault allows you to store any sensitive data securely and can be used with all your configurations. It also **integrates with cloud vendors like AWS, Azure, and GCP to allow for easy configuration across all your cloud vendors**.
- Vault allows you to store any sensitive data securely and can be used with all your configurations. This is especially helpful when your infrastructure configurations get large and complicated.
- Terraform Cloud features/benefits
  - Cloud cost estimation
  - Private Terraform module registry
  - Remote Terraform execution
  - Remote state management
  - Integration with repositories like GitHub and BitBucket, but **it does not have its own version control built in**
- Terraform Cloud - includes easy access to shared state and secret data, access controls for approving changes to infrastructure, detailed policy controls for governing the contents of Terraform configurations, and more.
  - Helps teams using TF together
  - Allows for easy access to shared state and secret data

## Other Resources

- [Exam Study Guide](https://developer.hashicorp.com/terraform/tutorials/certification-003/associate-study-003)
- [Exam Review Guide](https://developer.hashicorp.com/terraform/tutorials/certification-003/associate-review-003)
- [Exam Sample Questions](https://developer.hashicorp.com/terraform/tutorials/certification-003/associate-questions)

Required knowledge for exam

- Basic understanding of public cloud (at least one major provider)
- Know security best practices (Sentinel and Vault provider)
- Know TF workflow very well - Write > Plan > Apply
- Know when to use TF commands
  - init, plan, apply, state, fmt, validate
- Solid knoledge of TF state mechanism
- Knowledge of HCL to be able to interpret snippets of code
- Know differences between TF OSS and Enterprise offerings
  - Enterprise = Sentinel, cost estimation, storage of state in TF cloud by default
  - Different pay tiers
  - Terraform OSS workspaces VS TF cloud workspaces; oss stores workspaces as subfolders of terraform.tfstate.d folder
- Hands-on experience
- Exam based on 0.12+ version of TF
- Exam has 57-60 questions, questions can be flagged from review
- 
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

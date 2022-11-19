# Terraform Associate Certification Exam

[Exam Review](https://learn.hashicorp.com/tutorials/terraform/associate-review)

## Exam Overview

### Exam description

- 1 hour
- 50 to 60 questions, v0.12+
- Online Proctored
- 2 years validity period

### Exam Objectives

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
- Expand your skills and knowledge about Terraform and best practices
- Get insights into the Enterprise features of Terraform
- Solidify Terraform fundamentals and kickstart your journey to more advanced deployments

## Understanding IaC

### IaC and Its Benefits

What is IaC?
- No more clicks - write down what you want to deploy in a human readable way
- Enables DevOps - codification of deployments means they can be tracked down in version control enabling better visibility and collaboration across teams
- Declare your infrastructure - infra is mostly written using declarative code but can be procedural(imperative) too
- Speed, reduced costs and risk - less human intervention during deployment reduces probability of securety flaws, unnecessary resources and saves time

Terraform code sample

```h
# Declare provider
provider "aws" {}

# Create VPC in us-east-1
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
- Automate software defined networking (SDN)
- Interacts and takes care of communication with control layer APIs with ease
- Supports a vast array of private and public cloud vendors (major cloud, cloud, network, infrastructure software, database etc.)
- Tracks state of each resource deployed

### Understanding IaC - Recap

**Cloud agnostic** = technology not bound to one cloud and can work in a similar fashion across other cloud environments. Terraform doesn't care what cloud or infrastructure deployment method you're using and it works seamlessly with a long (and growing) list of cloud platforms.

IaC enables organizations to have better DevOps practices by tracking infrastructure code and deploying it in a repeatable, predictable manner. IaC is a great way to enable quick deployments and collaboration across teams.

Terraform uses its own language known as **HashiCorp Configuration Language (HCL)**.

IaC is a method of writing human-readable code to deploy resources in the cloud and elsewhere. IaC = code that deploys your infrastructure resources onto various platforms instead of managing them manually through a user interface.

## IaC With Terraform



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

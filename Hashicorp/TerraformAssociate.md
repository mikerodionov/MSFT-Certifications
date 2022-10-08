# Terraform Associate Certification Exam

[Exam Review](https://learn.hashicorp.com/tutorials/terraform/associate-review)

Exam Objectives:

1. Understand Infrastructure as Code (IaC) concepts
    1-a. Explain what IaC is
    1-b. Describe advantages of IaC patterns
2. Understand Terraform's purpose (vs other IaC)
    2-a. Explain multi-cloud and provider-agnostic benefits
    2-b. Explain the benefits of state
3. Understand Terraform basics
    3-a. Handle Terraform and provider installation and versioning
    3-b. Describe plug-in based architecture
    3-c. Demonstrate using multiple providers
    3-d. Describe how Terraform finds and fetches providers
    3-e. Explain when to use and not to use provisioners and when to use local-exec or remote-exec
4. Use the Terraform CLI (outside of core workflow)
5. Interact with Terraform modules
6. Navigate Terraform worklfow
7. Implement and maintain state
8. Read, generate, and modify configuration
9. Understand Terraform Cloud and Enterprise Capabilities

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

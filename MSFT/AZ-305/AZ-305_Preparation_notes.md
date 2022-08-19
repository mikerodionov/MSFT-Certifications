# AZ-305 Designing Microsoft Azure Infrastructure Solutions - Preparation Notes

## AZ-305 Exam & Architecting Azure Solutions

### Objective Domains

- Design identity, governance, and monitoring solutions
- Design data storage solutions
- Design business continuity solutions
- Design infrastructure solutions

### Exam Focus

- Arcihtecture - designing resilient, performmant and secure solutions in Azure
- Products - how to pick the right product/feature for the task (groups/types: identity, inftastructure, apps, migrations, governance)
- Pricing - key licensing and pricing tiers (when AAD P1 premium license is required, Azure Functions consumption plan VS premium pricing, Azure SQL general purpose VS business critical tiers etc.)

Many important security features leverage AAD premium licenses (priveleged identity management, dynamic groups etc.), it is possible to get 1 free month trial of those.

Poducts and features can be divided by groups: identity, infrastructure, apps, migrations, governance

Fundamentals > Adminisrtration > Architecture

AZ-305 prerequisites: fundamentals & administration, AZ-305 focus: architecture

Fundamentals - basic Azure knowledge, IT operations, and infrastructure
Administration - Azure & on-premises administration experience
Architecture - designing Azure solutions that meet specific requirements

Exam covers massive range of services. Some of those services/features require additional licensing costs.

### Solution Architect Role

[Well-Architected Framework](https://docs.microsoft.com/en-us/azure/architecture/framework/)
[Architecture Patterns](https://docs.microsoft.com/en-us/azure/architecture/)

Hands-on architecture VS enterprise architecture (differs in degree of low level details defined by an architect)

Common responsibilities for the role:

- Initial requirements gathering - discussions with customer
- Solution scoping and pricing (statement of work/SOW) - can vary from simple to advanced/complex, hence free or paid - approved by customer
- Detailed solution design - discussions with project sponsor and key stakeholders, workshops & discovery sessions; confirming and clarifying goals and constraints
- Solution implementation - by separate team with oversight of architect normally
- Handover to operations

AZ-305 exam focuses on services of Azure which can be used to design solutions based on requirements/constraints.

Azure Well-Architected Framework - a set of architectural pillars, which helps designing a modern cloud solution (securitym reliability, cost, operations, performance)
Azure Architecture Center - a variety of patterns, practices, and guidelines for architecting solutions on Azure; includes netwroking, hosting, applications, etc.

### Cloud Adoption Framework

[Microsoft Cloud Adoption Framework](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/) aka CAF - set of steps to achive success with Azure.

1) Strategy - What is success? What are the goals of this journey?
2) Plan - Understand how/plan next steps
3) Ready - Prepare the environment for deployment (and production)
4) Adopt - Implement the solution

+ Governance should be taken into account throughout Plan/Ready/Adopt stages - policies and processes to secure, control, and comply.
+ Manage - continoully manage, monitor and optimize

Adopt phase types/options:
- Migrate - migrate and modernize workloads for the cloud
- Innovate - Improve and expand skills and capabilities

#### Adopting the cloud - Migration

Assess - evaluate workloads (apps, infra, data) to be migrated to Azure
Deploy - prepare the environment, and migrate workloads to Azure
Release - optimize, monitor, and handover solution to ops

Tools

Service Map (Azure Monitor tool) - Maps communication between app components on Windows or Linux
TCO Calculator - Estimate monthly Azure running costs compared to on-premises
Azure Migrate service - Tools for assessment and migration of machines, data, web apps, and more to Azure
Cost Management - Native Azure cost monitoring and reporting (identify where you spend more - regions, services etc.)
Azure Advisor - Personalized recommendations ranging from performance to cost for your Azure resources
Azure Monitor - Centralized, multi-faceted monitoring platform for both Azure and on-premises resources

#### Adopting the cloud - Innovation

Build - Identify an opportunity and build a MVP
Measure - Verufy whether your opportunity was realized
Learn - Gather feedback, and adjust the innovation strategy

Expand Digital Innovation: Democratize, Engage, Empower, Interact, Predict

## Design Identity and Access Management

- Key concepts
- AAD
- Hybrid and External Identities
- Access Control

### Key Concepts

#### Traditional On-Premise Architecture vs Modern Architecture

Traditionally security was base on the network perimeter. With internal subnets/networks (servers/users networks etc.) inside of the on-premise network perimeter and behind the firewall. On-premises is our boundary within which everything is trusted.

Cloud changed the resource hosting and access model with remote staff, SaaS, and cloud deployments instead of or in addition to on-premises staff and resources. On-premises no longer is our trusted perimeter but just another resource we can access from different locations. Because of this, modern solutions are now designed with identity at the center.

Modern solutions have identity at the center. AAD helps to implement this.

#### Authentication and Authorization

Identity platforms facilitate authentication and authorization, providing identities to applications and users to gran access to resources/services.

Authentication/**AuthN** - verify that user/app is what it claims to be (password etc.)
Authorization/**AuthZ** - verify that user/app has access/permission to resource/action

### AAD


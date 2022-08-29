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

Traditionally security was based on the **network perimeter**. With internal subnets/networks (servers/users networks etc.) inside of the on-premise network perimeter and behind the firewall. On-premises is our boundary within which everything is trusted.

Cloud changed the resource hosting and access model with remote staff, SaaS, and cloud deployments instead of or in addition to on-premises staff and resources. On-premises no longer is our trusted perimeter but just another resource we can access from different locations. Because of this, modern solutions are now designed with **identity at the center**.

Modern solutions have identity at the center. AAD helps to implement this handling authentication and authorization.

#### Authentication and Authorization

Identity platforms facilitate authentication and authorization, providing identities to applications and users to gran access to resources/services.

Authentication (**AuthN**) - verify that user/app is what it claims to be (password etc.)
Authorization (**AuthZ**) - verify that user/app has access/permission to resource/action

### AAD

- Azure AD Tenants
- Identities
- Groups

#### Azure AD Tenant and Azure Subscription Associations

Azure subscriptions must be associated with Azure AD.

- Azure Active Directory tenant/ domain.onmicrosoft.com, you can have tenant for other MSFT services without subscriptions
- Subscriptions - associated with AAD tenant so that you grant access to resources within those subscriptions to your AAD identities

AAD tenant can be associated with multiple Azure subscriptions, but every subscription must be associated with one AAD tenant.

#### AAD identities

Azure supports identities for different scenarios - remote staff & on-premises staff accessing on-premises/Azure/SaaS resources.

AAD tenant identities:

- User accounts - represent staff member within an organization, use credentials (username & password); can be cloud, synchronized or guest users
- Applications (service principals) - represent an application in use within the tenant, use a secret token or certificate for authentication, can be used for apps running in Azure or elsewhere
- Managed Identities - represent a service within the Azure subscription, leverages the Azure platform for authentication, **only supports services running in Azure**

**Managed identities** allow identities to be created within Azure AD for supported Azure resources. The Azure platform facilitates authentication against Azure AD for the given resource. Most importantly, this means that the **resource does not have to have credentials stored locally**.

#### AAD Groups

Groups reduce the effort for access management/administrative overhead - we organize AAD identities into groups.

Best practice is to always perform permissions assignments via groups and never do direct assignments to identities.
Security groups - are AAD groups we use to assign permissions, for management MSFT 365 other type of groups is used (MSFT 365 groups).

There are 2 types of AAD groups:
- Security
- Microsoft 365

Main types of group membership:

- Assigned - admin/owner determine membership, membership is manually managed by admins or owners, requires the most effort to manage
- Dynamic - device/user attributes determine membership, membership is manages by platform and no manual changes allowed, **requires Azure AD Premium P1 licensing**

Dynamic user group membership **can only include user objects**. User group membership is based on a query that looks specifically at user properties (for example, Department, Company, Location). Dynamic user groups rely on dynamic membership rules to control group membership. The platform manages membership by using these rules. The rules help to minimize administrative overhead by looking at user properties to determine whether a user should be a member of a specific group.

You can enable group assignment to Azure AD roles at group creation stage ("Azure AD roles can be assigned to the group" option) = membeship type Assigned, but if you disable this you can select one of 2 other membership types - Dynamic User, Dynamic Device.

Memmbership types (set/configured on group creation stage) for security groups:
- Assigned
- **Dynamic user** - you will need to configure AAD dynamic query to configure attributes which will define group membership (AND/OR, property, operator, Value), e.g. based on specific department
- **Dynamic device** (based on device type)

Tenant - a representation of an organization within the AAD.
AAD objects - each tenant contains AAD objects (users, groups, apps, etc.)
AAD - acts as a central identity management platform

New user:
- Create user
- Invite user (AAD B2B)

Revoke access from users - after disabling access, also use Revoke sessions option (to kill active/existing sessions).

Soft delete preserves deleted user for 30 days (depending on configuration).

### AAD and Hybrid Idenities

- Azure AD Connect
- Azure AD Domain Services

#### Hybrid Identities

Extendinf identity and access into the cloud (legacy resources/on-premises identities).

We can synchronize on-premises identities into AAD using Azure AD Connect > sunchronized accounts within AAD

We can use Azure AD DS managed tomain in case legacy AD DS features are required > managed domain within AAD

If you use bot Azure AD Connect & Azure AD DS managed domain, synchronized identities will also sync into on premise managed domain.

Benefits of hybrid identities:

- Simplify the user login experience by providing seamless single sign-on
- Reduce management overhead
- Maintain identity as the perimeter of security
- Support legacy identity and service authentication and control

#### Azure AD Connect

AAD Connect is used for synchronizing identities & facilitates single sign-on for accessing resources.
AAD Connect can alter where authentication occurs (on-premises AD forest/AAD tenant)
AAD Connect methodds - Azure AD Connect sync & Azure AD Connect cloud sync (newer option)

Azure AD Connect provides various methods to synchronize identities between a traditional Active Directory forest and an Azure AD tenant. There are different methods for controlling where and how authentication occurs also (e.g.,**pass-through authentication, password hash sync**, or **ADFS**). All of this is to help facilitate seamless single sign-on to Azure AD registered resources so that users do not have to remember two different sets of credentials.

#### Azure AD DS

- Some legacy resources may depend on traditional AD Domain Services features
- AAD does not provide one-to-one feature parity with AD DS
- AAD DS Managed Domain - supports domain join, group policy, LDAP, Kerberos/NTLM authentication
- With AD DS Managed domain we have one way sync from AD DS into AAD DS Managed Domain + Legacy AD DS features

Azure AD Domain Services (Azure AD DS) is used to provide traditional Active Directory functionality such as domain join, group policy, LDAP, and Kerberos/NTLM authentication. This is provided through an Azure AD DS managed domain that is synchronized with Azure AD. This allows resources/applications/services in the cloud to access traditional Active Directory (AD) functionality without having to deploy AD Domain Services to VMs in the cloud. Note: make sure to differentiate between AD DS (traditional/on-premises) and Azure AD DS (cloud-based service).

### AAD External Identities

External Identities help to provide customers and partners with access to resources.

- AAD Business-to-Business (B2B) - invite partners from their identity providers
- AAD Business-to-Consumer (B2C) - access with 3rd party identity providers (LinkedIn, Facebook, GitHub, Twitter) through special tenant (Azure B2C Tenant)

MSFT is currently merging these 2 options into Azure AD External Identities - progression and combination of both AAD B2C and AAD B2B.

#### AAD B2B

Supported external partner identities:
- Work/school accounts
- Gmail
- Facebook
- SAML
- WS-Fed identities

Partner users **invited** as guests - that provides seamless, licensed access to your organization's resources.

Once users are invited, the rest is similar to normal user accounts - partner users are discoverable, and can be managed by access reviews.

When we invite partner identity, we later grant consent to sign it in and read its name/email/photo.

#### AAD B2C

Usef to provide access to custom applications.

- B2C Tenant - identity information is stored within a dedicated B2C tenant
- Supported identities:
  - Social identities
  - Local
  - SAML and WS-Fed-based identity provider federation
- Users are self-managed and private (identities are managed by users themselves), branding can be customized (user flows)
- AAD B2C supports more identity providers than AAD B2B

AAD B2C configuration:
- Create tenant
- Switch to tenant (Switch directory)
- AAD B2C - register an application, add identity provider(s), create a user flow (define the experience for users signing up and signing into your organization)

Azure AD B2C is an identity access management platform built on top of Azure AD. Developers can create an Azure AD B2C tenant dedicated to their application(s). Application users can then bring their own identities (using supported social IDs), or simply have a local account just for the app. Developers who use Azure AD B2C to facilitate access to their application can rely on Microsoft for managing the infrastructure, scalability, and connectivity of an identity platform, instead of having to do so themselves.

### Azure Access Control

- Azure RBAC
- Azure AD roles
- Custom Roles

#### Azure RBAC

Security principal + Role Defintion (built-in/custom) + Role Assignment (scope/resource - Management group/Subscription/RG/specific resource)

Built-in roles: owner, contributor etc.

Best practice assign roles to security groups instead of individual users.

#### AAD Roles

Security principal + Role Definition + Scope (AD Tenant, Administrative Unit, Application)

AAD > User > Assigned roles > Add assignments: select role (built-in/custom - AAD Premium P1 or P2 is required for custom roles assignment) + scope

#### AAD RBAC Custom Roles (JSON definitions)

- Used to follow the least privelege principle when built-in role provides too much access
- Leverages actions/dataActions and notActions/notDataActions to define permissions (ALLOW model)
- Available to all subscriptions within the same AAD tenant

AAD RBAC roles for Azure resources are separate from Azure AD roles. AAD RBAC roles define permissions for operations on Azure resources.

An Azure RBAC role defines the set of actions and data actions allowed (or denied) to be performed on Azure resources. These permissions are specific to Azure resource operations, and are entirely separate from Azure AD operations.

#### AAD Custom roles (no JSON definition)

- The same use case - Used to follow the least privelege principle when built-in role provides too much access
- Can only specify the permissions that will be allowed by the custom role (currently no DENY functionality)
- Available to all subscriptions within the same AAD tenant

AAD RBAC roles for Azure resources are separate from Azure AD roles. AAD roles define permissions for operations on Azure AD.

### Design for Identity and Access Management

- Custom app modern authentication (multiple identity providers, local accounts) > AAD B2C Tenant
- 3rd party users from other AAD tenants > AAD B2B + Custom RBAC role if necessary

## Design for Identity Security

- Protecting identities
- Conditional security
- Securing privileges
- Reviewing access

Identity security is not only assigning permissions to users. We need to decide:
- Do permissions needed now?
- Are there risks? Misuse of permissions, compromised accounts
- Are additional controls required? Access from untrusted/unusual locations

### AAD Identity Protection

Requires AAD Premium P2 licensing, helps to mitigate identity related risks:

- Credentials theft
- Use of duplicade passwords/re-use of the same password
- Use of unapproved applications

Azure AD Identity Protection - protects identities from being compromised **by detecting risks** (Detection > Risk Analysis > Integration):

- automates detection and remediation of identity-based risks
- allows easy access to risk-related data and reports within the Azure portal
- allows to export data and integrate with security monitoring tools
- **requires AAD Premium P2 licensing**

Configuration - Sign-in Risk Policy, User Risk Policy

Risk Events - risks that Identity Protection is monitoring proactively and reactively:

- Users with leaked credentials (MSFT looks up for leaked credentials on Internet)
- Sign-ins from anonymous IP addresses
- Impossible travel to atypical locations
- Sign-ins from infected devices
- Sign-ins from IP addresses with suspicious activity (anormal location change)
- Sign-ins from unfamiliar locations

Based on risks we can block login or block an account entirely.

Configuration of Azure AD Identity Protection involves configuration of Sign-in Risk Policy, User Risk Policy. We select risk levels out of High/Medium/Low and avobe - we do not have more detailed configuration/insight as to what exactly it implies.

#### Sign-in Risk Policy

Sign-in Risk Policies - define what action to take if risk has been found to be associated with an identity

Sign-in Risk Policy implies:

- **Real-Time/Online Detection** - takes effect in real time, these policies can be used to block a sign-in as it occurs
- **Assignment** - describes when the policy will trigger, this includes defining the applicable users/groups and the risk level condition (low/medim/high)
- **Control** - describes what to do when the policy triggers, the action can be block or allow **sign-in**, or allow access but require MFA

Configuration involves:

- Select/create policy
- Select applicable users/groups
- Define risk level
- Define control action (allow access, block access, allow but require MFA)

#### User risk policy

**Offline detection** - takes effect offline, these polices can identify user accounts that are found to be at risk
**Assignment** - describes when the policy will be triggerd, including definition of the applicable users/groups and the risk level condition
**Control** - describes what to do when the policy triggers, the action can be block or allow **accesss/account**, or allow access but force password change

#### MFA registration policy

Also included into Identity Protection and allows to eforce multi-factor authentication registration policy.

Identity Protection includes reporting and alerts on detected risks.

Idenity Protection requires AAD Premium P2 license.

### AAD Resources Protection - AAD Conditional Access

AAD Conditional access offers security controls and restrictions that are tailored to different scenarios.

**Requires AAD Premium P1 licensing.**

- Flexible security controls - we can configure different rules for different scenarios
- Leverages varios signals - variety of signals can be checked, including a user's location, risk level, etc.
- Can enforce different controls - we can require users to meet special conditions before granting access

Signals: Location, Country, Device, Browser, Risk (risk level from AAD Identity Protection if you have AAD Premium P2 license required for it)

Requirements/configuration:

- AAD tenant with AAD Premium P1 licensing
- Access Policy Assignment - users/groups this policy applies to, and cloud app or action being accessed; it is also possilbe to include the conditions under which access is being requested (signals)
- Access Policy Access Controls - define what happens if contditions are met (block/allow access, report-only mode for auditing purposes); make sure you not lock out your admins
- Use **Report-Only mode** or the **What If tool** to evaluate policies (when multiple policies are configured it is not so easy to see the final effect)

Conditional Access settings in Azure Portal:

- Named Locations: We can create Named Locations (e.g. by countries) using IP v4 or v6 ranges and set them as Trusted (trusted/non trusted flag can be used as a signal for conditional access policies)
Creating New Conditional Access Policy:
- Assignments - Users/workload identities - Include (none/all users/select users and groups)  with optional **Exclude configuration** (all guest and external accounts/directory roles/users and groups) which you should **use to exlude your admin or emergency accounts** (to avoid locking out your admins out of Azure Portal)/Clod apps or actions/Conditions/Controls
- Assignments - Cloud apps or actions - None/all cloud apps/select apps
- Assignments - Conditions (extra signals)
  - User risk (Identity Protection risk level) - High, Medium, Low
  - Sign-in risk (Identity Protection risk level) - High, Medium, Low, No risk
  - Device platforms - Any Device or Selected Platfroms - Android, iOS, Windows Phone, Windows, macOS, Linux
  - Locations - All trusted locations or Selected Locations
  - Client Apps - Browser, Mobile apps and desktop clients, Legacy Authentication clients - Exchange ActiveSync clients, Other clients
  - Device state (Preview)
  - Filter for devices - Property/Operator/Value to filter on very specific device property (DeviceId, DisplayName, OperatingSystemVersion, Model, Manufacurer etc.)
- Access controls
  - Grant
    - Block access
    - Grant access but require one or all of the selected possible controls
      - Require MFA
      - Require device to be marked as compliant
      - Require hybrid AAD joined device
      - Require approved client app
      - Require app protection policy
      - Require password change
  - Session - provide limited experiences within specific cloud applications
    - Use app enforced restrictions (only works with supported apps - Office 365, Exchange Online, SharePoint Online)
    - Use condtional access app control
    - Sign-in frequency
    - Persistent browser session
    - Customize continuous access evaluation
    - Disable maintenance defaults (Preview)

### Protecting Priveleges - AAD Privileged Identity Management (PIM)

Protecting privileges for AAD and Azure roles.

- Protect - enforce additional workflow-like tasks for users to be provided with their priveleges
- Audit - provide an audit trail with information on privileged usage and justification
- Review - simplify and automate the ability to review whether privileges are still require by users

Problems/scenarios

- Security admin in theory may need global admin priveleges, but for sure he does not need them 24 hours a day/all the time
- Some permission need only for specific period of time - project/activity - access reviews may help with that

AAD PIM acts as a gate between users and AAD/Azure privileges, providing the following features

- Just-in-time access - privileges are only assigned but not activated until they are required
- Time-bound access - privileges are deactivated after a set period of time using start/end dates (project timeline, contractor contract timeliene etc.)
- Approval - users who are assigned privileges must request approva before they will be activated
- MFA - enforce the use of MFA to activate any role

AAD PIM also enables audit and review of privileges

- Justification - require justification to be included in any requested activation of privileges
- Notification - receive notifications when any privileged roles are activated
- Audit history - download and access logs that detail all Privileged Identity Management Activities
- Access reviews - periodically review access to ensure users only have roles they currently require

Deploy & configure PIM

- PIM requires AAD Premium P2 or EMS E5 licensing
- PIM is automatically enabled for an organization when a user with a privileged role first accesses it
- Configure role settings for Azure and AAD roles (Azure requires role discovery)

When PIM is enabled in AAD User options for Assigned roles insted of Assigned Roles, you see more options there:

- Eligible assignments - Role, Principal name, Scope, Membership, State, Start time, End time, Action
- Active assignments - Role, Principal name, Scope, Membership, State, Start time, End time, Action
- Expired assigments - Role, Principal name, Scope, Membership, State, Start time, End time, Action

PIM > Manage

- AAD roles
- Privileged access groups (Preview)
- Azure resources (Refresh/Discover resources/Activate role)

PIM > Quick Start

Assign > Activate > Approve > Audit

Assignment - Eligible (with time frame) /Active (with time frame)

Roles support multiple PIM settings
- Activation maximum duration (hours), default 8
- Require MFA on activation
- Require justification on activation
- Require ticket information on activation
- Require approval to activate

User who has assigmnent then goes to PIM > My roles and is able to see and activate his assignments listed under Eligible assignments

### Designing Identity Governance

Access reviews for removing privileges where they no longer required.

- Review - manage and conduct access reviews for groups. apps, and roles for staff and guests
- Schedule - schedule reviews to be performed on a regular basis as part of good identity governance
- Automate - automate changes (deny/approve) to access based on the outcome of a review

Access review implementation:

- Create the review - specify the review type and timing (e.g. monthly group review)
- Start the review - the review will begin, and all configured reviewers can perform the assesment
- End the review - access will be updated based on the results of the review (can be automated)
- Iteration - start next month review/end next mont review etc.

Important considerations/components:

- Requirements/prerequisites
  - Requires AAD P2 licensing for reviewers
  - Azure resources must be discovered

- Azure Portal Capabilities
  - Configure access reviews
  - Review/apply access review results: group/applications reviews done via **AAD Identity Governance**, role access (AAD or Azure RBAC roles) reviews done via **PIM**

- Access Panel Capabilities
  - Separate UI for reviewers
  - Allows for responses (like justification)

Access review configuration
- Review type
  - Teams + Groups
    - Review Scope: All MSFT 365 groups with guest users / Select Teams + groups
    - Scope - Guest users only / All users
  - Applications
- Reviews
  - Specify reviewers - Group owner(s) / Selected userI(s) or group(s) / Users review their own access / Managers of users
  - Specify recurrence of review
    - Duration (in days)
    - Review recurrence - One time / weekly / Monthly / Quarterly / Semi-annually / Annually
    - Start date
    - End date - Never / Specific date / After number of occurrences
- Settings
  - Upon completion
    - Auto apply results to resource
    - If reviers don't respond - No change / Remove access / Approve access / Take recommendations
    - At the end of review send notification to - select user(s)/groups(s)
  - Enable reviewer decision helpers
    - No sign-in within 30 days
  - Advanced settings
    - Justification required
    - Email notifications
    - Reminders
    - Additional content for reviewe email

Review types recap

- Group membership
  - Reviewer type - Specified reviewers, group owner, self-review
  - Review creation - **AAD access reviews, AAD groups**
  - Reviewer experience - Access panel
- App assignment
  - Reviewer type - Specified reviewers, self-review
  - Review creation - **AAD access reviews, AAD enterprise apps**
  - Reviewer experience - Access panel
- AAD Role
  - Reviewer type - Specified reviewers, self-review
  - Review creation - **AAD PIM**
  - Reviewer experience - Access panel
- Azure Resource Role
  - Reviewer type - Specified reviewers, self-review
  - Review creation - **AAD PIM**
  - Reviewer experience - Access panel

### Design for Identity Security - Requirement/Solution + Recap

- Enforce MFA when app accessed outside of corp network - conditional access policy (enforce MFA but exclude corp network/head office - trusted/known location), conditional access requires AAD P1 license
- Block access when Identity Protection identities risk - use Identity Protection risk factor within conditional access policy (IP requires AAD P2 license)
- RBAC - to follow/implement principle of least privileges
- Just-in-time privileged access manageemnt - PIM (activate priveleges for N of hours), requires AAD P2 license
- Elevation approval - PIM, requires AAD P2 license
- PIM & IP require AAD P2 license

Conditional Access location, when configured can be excluded on a per-policy bases. Configured based on IP address ranges, and can be tagged as a trusted location. Once location is configured it can be used in zero or more policy to be included or excluded. You can also configure country location (in this case you select geograhic location & MSFT manages the IP addresses associated with it to define whether the request originates from a specific country).

[AAD Identity Protection](https://docs.microsoft.com/en-us/azure/active-directory/identity-protection/overview-identity-protection) requires AAD Premium P2 licensing

An **IP sign-in risk policy** can be used to block sign-in attempts as they occur based on risl levels associated with authentication request based on anonymous IP address being used, sign in from distant/atypical locatios, from malware-linked IP addresses, etc.

Access Reviews
- Review users assigned to AAD integrated apps
- Review group memberships (cloud-based, synchronized and Microsoft 365 groups)
- Review AAD and Azure resource roles

AAD Conditional Access requires AAD Premium P1 license

AAD Conditional Access allows require conditions (check/enforce) for different scenarios. Acccess can be blocked/allowed for specific scenarios or under specific conditions - e.g. for specific devices, locations, risk levels, etc.

It is possible to configure PIM to require MFA for role activation. Global Admin role settings can be configured to require MFA for all activations.

What If feature of conditional access policies
- Helps determine whether access would be allowed or denied when multiple policies are configured
- Allow you to specify the conditions and parameters of a given scenario to determine the policy result

Multiple Conditional Access policies can be configured and all conditions of all policies that apply to a given access scenario must be met. The What If tool helps in determining how multiple policies impact access to resources. With the What If tool, you are able to enter in all of the various parameters for a given scenario (IP address, device type, risk level, etc.). Using this information, the tool will then determine the impact of all applicable Conditional Access policies that have been configured (e.g., will access be allowed/denied, would MFA be required, etc.).

AAD IP - protects us4ers and sign-ins from various risks, such as suspicious activity, leaked credentials, etc.

AAD PIM - can be used to ensure that both AAD and Azure RBAC roles are only activated when needed. With PIM, priveleges can be assigned, but not active. Additional features like just-in-time access to privileges, approvals, and justification can be configured.

Access reviews:

- Can be scheduled to run automatically on a recurring basis
- Can automatically remove access once a review is complete

Access reviews can be scheduled to recur periodically, such as weekly, monthly, quarterly, or annually. For example, a review could run on a monthly basis and be configured to allow a 2-week window within that month for the reviewers to participate.

Automation is one of the key benefits of access reviews. It is possible to have the results of a review automatically applied to a resource once the review finishes (e.g., automatically removing users from security groups they are a member of if they don't respond to a self-review access review).

## Design a Compute Strategy

- VMs
- Container Solutions
- Application Hosting
- Large-Scale Compute

Hosting & Shared Responsibility Models

IaaS - Greater control and access from the OS up; client responsible for OS, runtime, apps, functions, whiole provider takes care of infra
PaaS - Less control and access alongside, lower admin overhead; client responsible for apps, functions, while provider takes care of infra, OS, runtime
FaaS (Functions as a Service) / serverless - limited control and access with a focus on individual functions; client manages functions, while provider takes care of infra, OS, runtime, apps

You select model based on workload you have, and path to the cloud may vary. General steps outline:

- Architect and plan
- Lift and shift
- Handover
- Rearchitect and optimize for cloud
- Handover and maintain

### Architecting VM based solutions



## Design a Networking Strategy

## Design Connectivity and Security

## Design Apps for the Cloud

## Design Security for Apps in the Cloud

## Design Data Platforms

## Design an Analytics Platform

## Design Security for Data

## Design Recovery and Resilience

## Design Migrations

## Design Governance

## Designing an Auditing and Monitoring Strategy
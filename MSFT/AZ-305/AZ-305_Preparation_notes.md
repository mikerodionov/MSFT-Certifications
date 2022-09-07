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

### Recommended pre-requisite courses/learning

- Azure Well Architected Framework
- Azure Administration

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

**Hosting & Shared Responsibility Models**

IaaS - Greater control and access from the OS level and up - your workload/may require this level of control
  - client responsible for   - OS, runtime, apps, functions
  - provider responsible for - infra
PaaS - Less control and access alongside, lower admin overhead
  - client responsible for   - apps, functions
  - provider responsible for - infra, OS, runtime
FaaS (Functions as a Service) / serverless - limited control and access with a focus on individual functions
  - client responsible for   - functions
  - provider responsible for - infra, OS, runtime, apps

You select model based on workload you have, and path to the cloud may vary. General steps outline:

- Architect and plan
- Lift and shift
- Handover
- Rearchitect and optimize for the cloud (optimize performance or cost)
- Handover and maintain

### Architecting VM based solutions

#### VMs

VMs key characteristics:

- Versatility - support for many scenarios, from new solutions to lift-and-shift migrations
- Full control - control of everything from OS level and up (access to Windows Registry or Linux configuration via /etc); remote management
- High maintenance - you are responsible for maintenance, patching, updates, security, and more

Creating VMs

- Name - not easily changeable/reconfigurable
- Region/zone - impacts available sizes
- Deployment into VNET, region/zone must be the same for VNET and CM
- NIC resource provides connectivity, VM can have more than 1 NIC (depending on VM family)
- OS & storage
  - Image - supports market place or custom
  - Disks - requires at least an OS disk

VM families

- General Purpose - balanced CPU-to-memory ratio, suitable for test/dev and small workloads
- Compute Optimized - high CPU-to-memory ratio, suitable for medium workloads
- Memory Optimized - high memory-to-CPU ratio, suitable for relational DB servers
- Storage Optimized - for workloads with high disk throughput and IO requirements
- GPU - specialized VMs for graphics rendering, or deep learning workloads
- HPC - high performance compute with powerful CPU and network (ML, high-performance parallel batch processing)

Create new VM

- Basics
  - Subscription
  - RG
  - Instance details
    - VM name
    - Region - can influence size options
    - Availability options - No infra redundancy required
    - Security type - Standard
    - Image
    - Azure Spot instance - Y/N
    - Size - various sizes available within each family which define N of CPUs,  RAM, N of data disks, max IOPS, temp storage size & price
  - Admin account
    - Username
    - Password
  - Inbound port rules
- Disks
  - Disk options - can be influenced by VM family, storage optimized families offer more options
    - OS disk type + redundancy
      - Premium SSD - prod and performance sensitive solutions
      - Standard SSD - web servers, light enterprise apps and dev/test
      - Shandard HDD - backup, non-critical, and infrequent access
    - Delete with VM - Y/N
    - Encryption at host - Y/N
    - Encryption type - (Default) Encruption at-rest with a platform-managed key
    - Enable Ultra-Disk compatibility - Y/N
  - Data disks
- Networking
  - VNET - must be in the same region as VM
  - Subnet
  - Public IP
  - NIC NSG - None/Basic/Advanced
  - Public inbound ports - None/Allow selected ports
- Management
- Advanced
- Tags
- Review + Create

#### VM Scale Sets

VM scale sets functionality

- VM-based = similar benefits (control level) and maintenance overheads (responsibility level)
- HA - provides additional features for HA
- Autoscaling - capabilities for scaling out to meet demand

VM scale set architecture/components

- Image and configuration - VMSS configuration is similar to VM config and supports custom images
- VMSS instances - instances are deployed to an **availability set/zone** based on VMSS configuration
- Autoscaling - supports availability and demand by scaling instances count in and out
- Deployment options - AZ / multi AZ within a region (VNET can span AZs within a region)

Create VM scale set

Basics

- VMSS name
- Region
- AZ - multiple AZs can be selected (Zone 1/Zone 2/Zone 4) to handle data center failure
- Orchestration
  - Orchestration mode
    - Uniform - optimized for large scale stateless workloads with identical instances
    - Flexible - achieve HA at scale with identical or multiple VM types
  - Security type
  - Instance details
    - Image
    - Azure spot instance - Y/N
    - Size
  - Administrator account

Networking

- VNET - into which VMSS instances will be deployed to
- Network interface
- Load balancer - Y/N
- Load balancing settings
  - LB options
    - Application gateway - an HTT/HTTPS web traffic LB with URL-based routing, SSL termination, **session persistence**, and web app firewall
    - Azure load balancer - supports all TCP/UDP network traffic, port-forwarding, and outbound flows
  - Select a LB/create new

Advanced

- Allocation policy
  - Enable scaling beyond 100 instances
  - Force strictly even balance arcoss zones
  - Spreading algorithm
    - Max spreading
    - Fixed spreading (not recommended with zones)
- Custom data - pass a script or config file into VM while it is being provisioned
- User data - Enable/Disable -pass a script or config file into VM which will be availabe throughout lifetime of the VM (do not use for storing secrets or passwords)

VMSS scaling

- Manual scale - specify instance count
- Custom autoscale
  - Autoscale setting name
  - Resource Group
  - Predictive autoscaling (Public Preview)
  - Default - Auto created scale condition
    - Scale mode
      - Scale based on a metric
      - Scale to a specific instance count
    - Rules - e.g. increase instance count by 1 when CPU usage is above 70%
    - Instance limits - minimum/maaximum/default
    - Schedule

### Architecting Container-Based Solutions

Containers definition
Azure Container Instances
Azure Kubernetes Service

#### Containers

Dockerfile - container definition which used to generate container image (containing everything that we need to run our app/workload).

```
FROM nginx:alpine
WORKDIR /usr/share/nginx/html
COPY ./index.html ./
```

Container image

Container registry - storage for container images

Container instance - instance image running on top of container engine (AKS, Docker)

#### Azure Container Instances

- Launch in seconds, billed on a per-second basis basis
- Suits only simple workloads - testing, development or running short-living processes
- Limited functionality - supports container groups and persistent storage only

Azure Container Instances Components/Architecture

- Container Instances - deploy a container from Azure Container Registry, or other public/private registry
  - When started container instance image runs on container instance compute
  - Container restart = back to original state of its image
- Persistent Storage - Azure file shares can be mounted directly to a container for persistent storage (to persist/share data)
- Container Groups - containers can be scheduled on the same host to share resources

Create Container Instance

Basics

- Subscription
- Resource group
- Container details
  - Name
  - Region
  - AZs
  - Image source - Quick start images/ACR/Docker Hub or other registry
  - Image
  - Size

Networking

- Networking type
  - Public - will create public IP for your CI
  - Private - will allow you to choose a new tor existing vnet for your CI (not available for Windows containers)
  - None - won't create a public IP/vnet

Creating container groups

- Done using acigroup.yml file

```Bash
az container create --resource-group ACI-RG --file acigroup.yml
```

E.g. you may want to create an ACI group for Wordpress comprised of 2 ACI - Wordpress & MySQL

#### Azure Kubernetes Service

- Orchestration - automatic pod/cluster scaling, node upgrades, etc.
- Docker support - supports Docker images from public/private repositories
- Persistent storage - leverage Azure Files or managed disks for storage
- AAD integration - configure RBAC for AKS leveraging AAD identities
- Virtual networking - inregrated VNet connectivity
- Monitoring - integrates with Azure Monitor for complere monitoring

AKS components/architecture

- Control Plane - core Kubernetes infrastructure, managed by Azure. This is **not charged** as part of AKS. = full Kubernetes cluster managed by MSFT
- Nodes - VMs (or Azure Container Instances) that run workloads. This is **charged**.
- Pods
- Networking - connectivity within a VNet using Kubernetes networking or Azure Container Networking interface

#### ACI vs AKS

ACI intended for simple containerized solutions, to run a simple container(s)
AKS intended for more advanced containerized solutions, to run solutions made up of multiple different tiers (processing, web, data, API); AKS helps with orchestrating all solution containers, load balancing, scaling, healing etc.

Create AKS

Create Kubernetes Cluster

Basics
- Kubernetes version
- API server availability
  - 99,9 - optimize for availability, available when at least 1 AZ is selected
  - 99,5 - optimize for cost
  - 99,95 - recommended for standard config, available when at least 1 AZ is selected

Node pools
- agentpool
- Enable virtual nodes - Y/N - allow burstable scaling backed by serverless ACI
- Enable VMSS - creates a cluster that uses VKSS instead of individual VMs, required for autoscaling, multiple node pools, and Windows support; enabled by default
- Node pool OS disk encryption

Authentication
- Cluster infrastructure - identity used by AKS to manage cloud resources attached to the cluster
  - Service principal
  - System-assigned managed identity
- Kubernetes authentication and authorization - used to control user access to AKS  cluster and what user is allowed to do
  - RBAC - Enabled/Disabled (Enabled by default)
  - AKS-managed AAD - enables AAD integration and allows to assign AAD admin groups for AKS cluster

Networking
- Network configuration
  - Kubenet
  - Azure CNI - select subnet and each pod on a node will be using an IP from selected subnet, can quickly exhaust subnet space if high number of pod per node is set (consider modifyng default values of pod per node for each node pool)
- DNS name prefix
- Traffic routing
  - Load balancer
  - Enable HTTP application routing - Y/N
- Security
  - Enable private cluster
  - Set authorized IP ranges
  - Network policy

### Deploy a container with ACI

Container Image - defines exactly what our container should look like, including operating system, files, etc.
Azure Container Instances (ACI) - allows us to quickly deploy an instance of a container, from a container image
Network Connectivity - once container is up it van be accessed via FQDN and public IP

DNS name label - label.region.azurecontainer.io

labcontainer1.centralus.azurecontainer.io

### Architecting Application Hosting in Azure

Azure App Service
Function Apps
Pricing

#### Azure App Service

- Infrastructure management - no need to patch, maintain, or manage underlying infrastructure
- HA - built-in support for HA
- Autoscaling - supports automated scaling in and scaling out
- Streamlined development - integrated tools to streamline development
- CI/CD - includes CI/CD features such as staging slots
- Azure integration - with AAD for auth, with VNETs for connectivity (depends on selected plan)

Azure App Service architecture/components

- App
  - Web: Java, Ruby, Node.js, PHP, Python, .NET
  - Mobile: iOS and Android app backends
  - API: REST-based HTTP/HTTPs web APIs
  - WebJobs: scheduled/triggered tasks

- App Service Plan - hosting environment that executes your; plan determines features/resources
  - Can be Windows or Linux based, which influences available languages
  - Shared multi-tenant service - you share some ingress and egress networking, as well as compute (depending on plan) with other customers

#### Azure App Service Pricing

- Shared - cheapear plans with fewer features; apps can run on the same compute as other customers
- Dedicated - only apps belonging to the app service plan run on these dedicated compute nodes; includes additional features
- Isolated - entirely dedicated and isolated to a customer's network; includes greater scale out capability

App Service Plan

- Scale up (App Service Plan)
- Scale out (App Service Plan) - manual/custom autoscale - similar to VMSS

Create a Web app

Basics
- Project details
  - Subscription
  - RG
- Instance details
  - Name (name.azurewebsites.net)
  - Publish - Code/Docker Container/Static Web App
  - Runtime stack
  - OS
  - Region
- App Service Plan
  - Select/create plan
- Zone redundancy - Enabled (minimum app service instance count will be 3)/Disabled

VS Code - deploy Web App into existing Web App in Azure

#### App Service extra functionality

- Authentication - "Easy Auth", allows to add an identity provider (Microsoft/Apple/Facebook/GitHub/Google/Twitter/OpenID Connect)
- Custom domains and SSL enforcement
- Backups
- Custom domains
- Deployment Center - allows to setup deployment workflow (CI/CD)
- Deployment Slots - multiple slots can be created (staging/prod switch aka blue-green deployment); deployments slots are live apps with their own hostnames, app content and configurations elements can be swapped between two deployment slots, including the production slot

#### Azure Fuctions

Simplicity VS Control

Azure Functions (FaaS/serverless) provider greater simplicity and lesser control if compared with App Service, Containers or VMs.

Solution stack up to bottom:

- Solution functions
- Solution
- OS
- Infra

Azure Function Apps architecture/components

Azure Functions
- Function app - hosting environment and plan
- Function - code to perform some function
- Hosting plan

Trigger
- Trigger - what causes function to execute (e.g., a timer or an HTTP request)

Bindings
- Binding - inbound and outbound integration (e.g., an inbound HTTP trigger and outbound blob)

Function Hosting

Key features to consider when selecting an app service plan

- Consumption - pay for execution only (time, memory); scaling is managed for you
- Premium - provides extra features: warm instances, VNet connectivity, unlimited execution, and premium sizes
- Dedicated (you will be actually using app service plan) - an app service plan, useful for custom images or existing underutilized plans

Create function

- Development environment
  - Develop in portal
  - VS
  - VS Code
  - Any editor + Core Tools
- Select a template
  - HTTP trigger (&name=john)
  - Timer trigger
  - Azure Queue Storage trigger
  - Azure Storage Bus trigger
  - Azure Service Bus Topic trigger
  - Azure Blob Storage Trigger
  - Azure Event Hub trigger

App Service VS Azure Functions

App Service - when you want fully fledged application solution in the cluoud and only focus on programming it without worrying about the infra, with CI/CD and LB
Azure Functions - when you just want a simple function which has to be performed in response to some trigger (HTTP request, message from an Azure Queue, Azure Storage event etc.)

### Stage a .NET Web App Using App Service Deployment Slots and Azure CLI

The Azure App Service includes deployment slots to help improve the way in which updates to your code can be deployed to production.
Staging slot can be used to test changes in your web app.

Web App
Deployment Slot
App Service Plan

- Deploy a simple .NET Core web application to a new web app in Azure App Service
- Make changes to your web application, and deploy these to a staging slot
- Perform a slot swap, so that your changes are promoted to production

```Bash
# Create a new web app, -o to output web app into specified folder
dotnet new webapp -o webapp
# Switch to web app dir
cd webapp
# Create and deploy web app - make sure RG exists, otherwise error will be a bit counter-intuitive
az webapp up -n <APPNAME> -g <RESOURCE_GROUP> --sku s1 --location "<LOCATION>"
# Create deployment slot
az webapp deployment slot create -n <APP_NAME> -g <RESOURCE_GROUP> --slot staging
# Add changes to app - edit index.cshtml with Code
code Pages/Index.schtml
# Publish the updated code
dotnet publish -o build2
cd build2
zip -r build.zip .az webapp deployment slot swap -n <APP_NAME> -g <RESOURCE_GROUP> --slot staging --target-slot production
# Zip Deploy to staging slot - sometimes fails and has to be re-run
az webapp deployment source config-zip -n <APP_NAME> -g <RESOURCEGROUP> --src build.zip --slot staging
# Swap Staging and Production Slots
az webapp deployment slot swap -n <APP_NAME> -g <RESOURCE_GROUP> --slot staging --target-slot production
```

For slots it is also possible to set traffic pecentage per slot - so that you gradually introduce users to new version and monitor the situation/reported issues.

### Architecting Large-Scale Compute

High-Performance Compute
Azure Batch
Azure CycleCloud

#### Why of Large-Scale Compute

Big problem requires team work/splitting it into separate parts and processing by individuals.

High-Performance Compute (HPC) solutions share a common architecture

- Application/Service with resource intensive/big **job** (encoding/decoding etc.)
- **Job scheduler** splits job into smaller tasks to be execute by separate compute nodes
- **Parallel execution** - compute nodes process tasks received from scheduler simultaneously, in parallel and output resultat to storage
- **Tightly coupled execution** - data processed by one node, then by other and another, etc. (not parallel)
- Frequently compute nodes may require high-throughput, low-latency connectivity (InfiniBand)

#### Azure Batch

Azure Batch key functionality

- **Fully managed** cloud HPC cluster and scheduling infrastructure
- **Built-in scheduler** - no cluster or job scheduler software to install/manage
- **Intended for Developers** - targeted toward developers, provides tools and an API for HPC jobs

Azure Batch components/architecture

- Batch Account - includes all necessary HPC infra, such as nodes/node pools and job scheduling (splitting job into tasks)
- Application/Service - customer's application/service, responsible for managing jobs through the batch API
- Storage account - can be used for storing job-related resources and files (in/out)

Create Batch Account

Basics
- Project details
  - Subscription
  - RG
- Instance Details
  - Account name
  - Location
- Storage account

Advanced
- Identity type - None/System assigned/User assigned
- Public network access - Enabled/Disabled
- Pool allocation mode - Batch service/User subscription
- Authentication mode

#### Azure CycleCloud

Azure CycleCloud Functionality

- **Easy deployment** - simplified provisioning and management of cloud/hybrid HPC
- **Bring your own scheduler** - supports third-party schedulers and file systems
- **For HPC admins** - helps admins to deploy familiar autoscaling HPC solutions

### Isolating Compute-Based Solutions

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
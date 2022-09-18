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

Big problem requires team work with splitting it into separate parts which processed by individuals in parallel with coordination between them, the same is applicable for big computational tasks.

{Problem, Workers, Tools}

High-Performance Compute (HPC) solutions share a common architecture

- **Application/Service** with resource intensive/big **job** (encoding/decoding etc.)
- **Job scheduler** - splits job into smaller tasks to be execute by separate compute nodes
- **Parallel execution** - compute nodes process tasks received from scheduler simultaneously, in parallel and output result to a storage (contrast with tightly coupled execution)
- **Tightly coupled execution** - data processed by one node, then by other and another, etc. (not in parallel)
- Frequently compute nodes may **require high-throughput, low-latency connectivity** (InfiniBand)

#### Azure Batch

Azure Batch key functionality

- **Fully managed** cloud HPC cluster and scheduling infrastructure
- **Built-in scheduler** - no cluster or job scheduler software to install/manage
- **Intended for Developers** - targeted toward developers, provides tools (SDK) and an API for HPC jobs

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
- Pool allocation mode - Batch service (all batch resources will be managed for you)/User subscription (all batch resources will be managed within your subscription - you will see related VMs etc.)
- Authentication mode

#### Azure CycleCloud

Azure CycleCloud Functionality - service that helps to deploy familiar HPC services

- **Easy deployment** - simplified provisioning and management of cloud/hybrid HPC
- **Bring your own scheduler** - supports third-party schedulers and file systems
- **For HPC admins** - helps admins to deploy familiar autoscaling HPC solutions

Azure CycleCloud Architecture

- CycleCloud Application - solution runs on a VM and contains various HPC management features; **VM with CycleCloud service** running on it.
- Cluster Management - deploy and manage schedulers (Slurm, LSF, etc.) or file systems (BeeGFS, NFS)
- Subscription Access - CycleCloud app **requires subscription access to manage resources**
- App/Service - customer application/service is responsible for managing jobs through the scheduler

Create Azure CycleCloud (Create a VM with CycleCloud image)

Plan (Azure CycleCloud 8.1)

Create a VM

Basics
- Project details
  - Subscription
  - RG
- Instance details
  - VM name
  - Region
  - Availability options - No infra redundancy
  - Security type - Standard
  - Image - Azure CycleCloud 8.1 - Gen1

Azure CycleCloud Subscription management portal (runs on CycleCloud VM, you can connect over public IP of this VM)

New Cluster
- Schedulers
  - Altair Grid Engine
  - Grid Engine
  - HTCondor
  - lsf
  - Microsoft HPC Pack
  - OpenPBS
  - Slurm
- Filesystems
  - BeeGFS
  - NFS
- Infra
  - Single VM

We have to create managed identity for Azure CycleCloud VM and grant to it Contributor rights over subscription.

#### Azure Batch VS Azure CycleCloud

AzureBatch - turn on/ready to go service
CycleCloud - an easy way to deploy familiar/existing HPC products

### Isolating Compute-Based Solutions

Multi-Tenant Services
Dedicated Hosts
App Service Environment
Isolating Containers

#### Multi-tenant services

Azure > Azure Hypervisors (shared) > Hosting Environment (shared) > Various Customers Apps

#### Dedicated Hosts

Key Functionality

- Dedicated infra - physical host infra is dedicated to a single customer (not shared)
- Greater control - greater manual control over maintenance (e.g. patching, reboots scheduling etc.)
- Hybrid licensing - allows leverage existing software license agreements

Isolating VNs with dedicated hosts

- **Dedicated host** - physical server in an Azure datacenter, supporting a specific family of VMs
- **Support for VMs and Scale Sets** - deploy VMs or VMSSs (Windows/Linux)
- **Host group** - group of one or more dedicated hosts, helps to control HA

Create dedicated host

- Basics
  - Project details
    - Subscription
    - RG
  - Instance details
    - Name
    - Location
  - Hardware profile
    - Host group (for grouping multiple dedicated hosts/managing HA)
    - Size family - Standard DASv4 Family Type 1, Standard DCSv2 Family Type 1, etc. - availability depends on selected region
    - Fault domain - N (number of physical rack where host is located)
    - Automatically replace host on failure - Y/N
  - Save money
    - Already have Windows Server licenses - Y/N
    - Already have SQL Server licenses - Y/N
- Tags

 **Fault domain** - Just like VM in a scale set or availability set, hosts in different fault domains will be placed on different physical racks in the data center. When you create a host group, you are required to specify the fault domain count. When creating hosts within the host group, you assign fault domain for each host.

**Billing starts immediately** after dedicated host creation - irrespectively whether you use it or not.

- Maintenance control
  - Enable/disable
  - Create new default config
  - Use existing config
- Instances
  - Add VM/VMSS to dedicated host

#### App Service Environments

Key functionality

- **Dedicated environment** - deployed to a VNET (and dedicated hosts) in your environment
- **High scale** - leverage greater scale-out limits for hyperscale
- **Secure access** - acces can be configured for either internal or external use

Isolating Apps with App Service Environment (ASE)

- Hosting - underlying hosts are multi-tenant, unless using dedicated hosts
- App Service Plan (ASP) - an app is deployed to an ASP, which is deployed to an ASE
- Network - deployed to a customer's VNET for private connectivity (App Service was initially focused on network isolation)
- Accessibility - can be accessed publicly (external ASE) or publicly (public ASE)

Creating App Service Environment (v2/v3)

v3 - is the latest version which supports bot network and host compute isolation

Creating App Service Environment v3

- Basics
  - Project Details
    - Subscription
    - RG
  - Instance details
    - App Service Environment Name - NAME.appserviceenvironment.net (INT) / NAME.p.azurewebseites.net (EXT)
    - Virtual IP
      - Internal - the endpoint is an internal load balancer (ILB ASE)
      - External - exposes the ASE-hosted apps on an internet-accessible IP address
- Hosting
  - Hardware isolation
    - Enabled - ASE hosted on 2 dedicated physical hosts
    - Disabled - ASE hosted on virtual isolated hardware
  - Zone redundancy
    - Enabled - ASE and apps will be zone redundant (minimum app service plan instance count = 3)
    - Disabled - ASE and apps won't be zone redundant (minimum app service plan instance count = 1)
- Networking
  - VNET
  - Subnet
- Tags

Once ASE is created you can deploy apps into it

- App Service plans
- Apps

Wnen creating web app you will be able to **select ASE from Region drop down**, the rest is the identical to creating new web app
App Service plan - you will be selecting from Idolated plans group (offers greater resources scale/quantity)

#### Isolating Containers

- Azure Container Instances (ACI) - instances are isolated, but share hypervisors; containers are isolated from each other unless container groups are used
- Azure Kubernetes Service (AKS) - shares hypervisors, but you can use your own VMs to run containers

Both ACI and AKS are services which run on top of multi-tenant hypevisors.

- Dedicated host - provides support for AKS and ACI (more recent addition to these services) - you will be selecting it through Region drop down during resource provisioning stage

For any compute service dedicated hosts option provides hypervisor level isolation.

### Design a compute strategy

#### Scenario/requirements > possible solution(s)

Need to use familiar HPC tools without re-training and use existing HPC scheduler (Slurm) > Azure CycleCloud (allows to deploy and manage schedulers - Slurm, LSF, etc. or file systems -BeeGFS, NFS)
Web app requires OS level customization > prevents us from going for PaaS solution (e.g. Azure App Service), and leave us only with VM based solutions
Tolerate DC failure > Use multi-zone VMSS (within VNET attached to a region and spanning all AZs within that region)
Mimimize operational costs > Configure VMSS autoscale

#### Recap

##### Comopute Isolation

**Azure Dedicated Host** allows a physical host to be dedicated to an individual customer. This ensures there are no workloads virtualized by the hypervisor other than those belonging to the customer. Dedicated Host is increasingly supporting other services also, such as App Service Environment v3, Azure Kubernetes Service, and Azure Container Instance.

**App Service Plan Dedicated tier** - the Dedicated compute tier (Basic, Standard, Premium plans) **runs apps on dedicated Azure virtual machines**. No other customers share this compute with you. However, it does still operate on shared hypervisors. This tier does **support VNet Integration**, but it **does not deploy directly to a virtual network**.

**App Service Plan Isolated tier** - an App Service Environment (ASE) can be used to deploye apps to to your VNET and a hypervisor dedicated to you (when combined with Dedicated Host), an ASE is deployed to App Service plans on the Isolated tier.

##### VM based compute

Reasons to use of VM based compute solutions:

- Need to migrate an existing app to the cloud having access only to installers and not to the original source code (with possible exception for containerized apps which will allow you to use ACI/AKS)
- Need for full control on OS level
- Need to perform lift-and-shift migration of non cloud-optimized solution

Deploying to Azure PaaS (e.g., Azure App Service and Azure Functions) requires having the original source code.

VMSS

- Can be deplpoyed across multiple AZs within a single region; a VMSS can be deployed across multiple AZs within a single region. This helps to protect solutions from data center (availability zone) failure.

AZ = DC

##### AKS and ACI

Reasons to use AKS instead of ACI:

- Your containerized solution requires full orchestration, self-healing, load balancinf, and dynamic scale

Reasons to use ACI instead of AKS:

- You have a containerized solution that requires persistent storage and per-second billing
- You have a containerized solution, but you don't require full-fledged orchestration

ACI is a **simple on-demand container execution environment** which does **support persistent storage** and **billed on a per-second basis**. ACI is designed for deploying a single pod of containers on demand, quickly without support for full-fledged container orchestration.

ACI can share network and storage when using container groups. **Container groups** allow a collection of containers to be scheduled on the same host. Containers in the container group share resources, local network, and storage volumes. **This is like a pod in Kubernetes**.

##### Azure App Service

Azure App Service Features

- Ability to schedule or trigger a program or script (WebJobs)
- Deployment slots for continous deployment
- Built-in authentication and authorization functionality (Easy Auth)

Azure App Service supports **WebJobs** - functionality which allows a program or script to be run by schedule or trigger; such tasks are commonly run in the background as part of an overall application solution.
**Deployment slots** allow developers to deploy an app to a staging environment, perform validation and testing, and then easily swap the staging slot to production when ready; **requires a Standard App Service plan or higher**.
App Service includes built-in authentication and authorization functionality (**Easy Auth**) which supports sign-in providers such as Azure AD, Facebook, and Google.

##### HPC - Azure Batch / Azure CycleCloud

Azure Batch key features

- Provides job scheduling capabilities to developers through SDKs and an API
- Provides access to large-scale parallel compute without having to worry about clusters or software installation

Azure Batch is an HPC job scheduler as a service; everything is managed and maintained for you by Microsoft. There is no job scheduler software to install/manage.

Azure CycleCloud = service for HPC which allows minimal changes to an existing on-premise HPS process/solution

**Azure CycleCloud** is aimed at administrators and users of existing HPC solutions. CycleCloud **supports common third-party HPC schedulers** (Slurm, LSF) and **file systems** (BeeGFS, NFS) and helps automate and manage the deployment of an HPC cluster to Azure. This eases the transition to HPC in the cloud by allowing existing tools, processes, and understanding to be used with a cloud-based service.

##### Azure Functions

Azure Functions VNET connectivity support

- Requires Premium plan - provides VNet connectivity and other features such as pre-warmed workers and greater CPU/memory.

Azure Functions plans:
- Consumption (Serverless)
- Functions Premium
- App service plan

## Design a Networking Strategy

### Intro

Key Concepts
Virtual Networks
Integrated Networks
Hybrid Networks
Service Networking

Virtual Networks

- Isolated Networks - secure, private, and isolated from other virtual networks; fully isolated from other VNETs by default
- Default Routing - intra-VNET traffic, and outbound Internet is routed = connectivity within VNET + outbound Internet connectivity
- Multi-Zone Deployments - supports zone-based resources within a region / regional service, spans AZs within a region - no need to explicitly pin it to AZs within a region

#### Hub-and-Spoke Network Arcitecture

Hub-and-Spoke VNETs - Common network design for sharing centralized network resources and access
Spoke VNETs connected to hub VNET via peering
If all shared resources are hosted in a hub VNET we can manage FW & ExpressRoute and other things from one central location - hub VNET
Essentially we centalize access and access control

#### Public Accessebility for Azure Services

- Many Azure services are built for public accessibility (KV, web, storage, SQL) = have public end points

### VNETs recap

- Routing
- Network Security Groups (NSGs)
- VNETs NAT

#### Custom Routes

VNET routing by default = connectivity within VNET + outbound Internet connectivity, otherwise isolated

Custom Routes examples

- Blocking Internet Access - using the None next hop type, we can block internet access
- Forcing Traffic via Another Address - using various next hop types we can force trafic to specific destination, e.g. route it through firewall
- Configuration - routes within a route table apply to associated subnets
  - subnet can have only one or no route table assigned, no mutliple route tables can be assigned to the same subnet
  - the same route table can be assigned to multiple subnets

0.0.0.0/0 - wildcard for all traffic

Route consist of

- Name
- Address prefix (which traffic it affects)
- Destination - VNET gateway/VNET/Internet/Virtual Appliance/None

Special Scenarios

- Automatic System Routes - system routes can be automatically generated (e.g., default routes, VNET peering, service endpoints)
- Border Gateway Protocol (BGP) - BGP can help manage dynamic routing (e.g., ExpressRoute or VPN); advertises existing routes across some existing connectivity (ExpressRoute or VPN)
- Matching Address Prefix Routes - order of precedence is Custom > BGP > System (from highest precedence to lowest); custom always win

#### Nentwork Security Groups (NSGs)

- Traffic filtering - priority-based allow or deny rules are processed only until the first match is found
- Default rules - have high humbers (=low priority so that you can overwrite them creating rules wiyh lower numbers)
 - **DenyAllInBound** - default DENY rule 65500, Any Port, Any Protocol, Any Source, Any Destination
 - **AllowAzureLoadBalancerInBound** - 65001, Any Port, Any Protocol, Source AzureLoadBalancer, Any Destination
 - **AllovVnetInBound** - 65000, Any Port, Any Protocol, Source VNET, Destination VNET
- Assignment - assigned to a subnet or NIC, we can allow something on VNET level, but then block it for particular VM on a NIC level; NSG attached to VNET is assigned/applied to all the devices within it

NSGs considerations

- Public IP addresses - standard public IP addresses block access by default, basic IPs do not
- Stateful rules - NSG allow rules will automatically allow reply traffic
- Default outbound access - the platform provides outbound internet access (without a public IP)

#### VNET NAT

- Shared outbound internet - replaces the need for individual public IP addressing for outbound connectivity (unlike default outbound access allowing control over public IP addresses)
- Public IP addressing - creates virtual NAT appliance with a single standard public IP/also supports public IP prefixes (pool of public IP addresses which will be used by your VNET devices)
- Configuration - one NAT can be **associated with one or more subnets within a VNET**

### Integrated Networks

VNET Peering
Service Endpoints
Private Link

#### VNET Peering

VNET routing by default = connectivity within VNET + outbound Internet connectivity, otherwise isolated

VNET peering provides secure connectivity between peered VNETs

VNET peering

- Fast, low-latency private IP connectivity
- Supports cross-subscription connectivity
- Supports cross-region connectivity
- Address spaces cannot overlap
- Does not support transitive routing - your route only can go from vnet to directly peered VNET not transiting another VNET in the middle (for that you would need FW or router appliance)

Configure VNET peering

VNET1, add vnet1-to-vnet2 peering
- Peering link name
- Traffic to remote VNET
  - Allow (default)
  - Block all traffic to remote VNET
- Traffic forwarded from remote VNET
  - Allow (default)
  - Block traffic that originates from outside this VNET
- VNET gateway or Route Server
  - Use this VNET's gateway or Route Server
  - Use the remote VNET's gateway or Route Server
  - None (default)

#### Service Endpoints

- Azure PaaS service have public endpoints by default
- Configuring service endpoints - configured per resource provider, per subnet, to provide secure connectivity
- System routes - optimal routes are added so that all resources within a subnet use the MSFT backbone withoug going through the public internet
- Network security - resource firewall can be configured to allow/deny traffic

Being added on subnet level by means of **selecting entire resource providers** (Microsoft.Storage, Microsoft.KeyVault). Once applied you can see routes in VM NIC effective routes UI - you will see VirtualNetworkServiceEndpoint routes there.

#### Private Link

Secure Netwok Connectivity

- Private Link Support
  - supported Azure services
  - customer/partner-managed solutions
- Injects Private Endpoint with private IP into your VNET which will act as a proxy for resource access
- **Granular security** - **configure connectivity to specific resources** (not a whole resource type)
- Broad Accessibility through VNET ingected private IP
  - Accessibility from on premises
  - Access from peered VNETs

Create Private Link
- Create private endpoint - for supported MSFT services
  - Resource within your directory or outside of it by resource ID or alias
  - Integrate with private DNS
- Create private link services - for customer/partner services

### Connect Hub and Spoke Networks with VNet Peering

**Hub and spoke** is a common network topology used to both isolate and interconnect networked resources securely.

- Hub network contains secure private-network resources, managed from HQ
- Spoke network used to provide remote management access (via jump host deployed into it)
- Network connectivity - VNET peering and NSGs used to control access

- Configure peering to allow access from spoke/jumper VNET to hub VNET
- Configure NSGs to provide only necessary access

- Configure 
  - VNet peering
  - Public IP addressing
  - network security groups to configure secure RDP connectivity from a spoke network to the hub network

Hub VNET NSG has deny-vnet-inbound rule.

Other options to implement similar functionality - **Microsoft Defender for Servers just-in-time VM access**, **Azure Bastion service**

### Hybrid Networks

VPN
ExpressRoute
Virtual WAN

#### Virtual Private Networking (VPN)

Providing private, encrypted connectivity to Azure virtual networks (encrypted tunnels) **across the internet**. Private connectivity over private IPs (but requires public IP for VPN termination point). You can also use VPN to arrange VNET to VNET connectivity, although VNET peering is used more frequently for that.

Connecting on-premises/remote users to Azure - **P2S (Point-to-Site) VPN**
Connecting on-premises/remote network to Azure - **S2S (Site-to-Site) VPN**

Secure/encrypted connection over the public internet.

VNET peering VS VPN

| VNET Peering                                                                    | VPN                                                                |
|---------------------------------------------------------------------------------|--------------------------------------------------------------------|
| - Designed for **VNET-to-VNET connectivity**                                    | - Designed for **hybrid connectivity (S2S, P2S)**                  |
| - Supports cross-subscription, cross-region, cross-Azure AD tenant              | - Supports cross-subscription, cross-region, cross-Azure AD tenant |
| - Leverages **MSFT backbone** for private IP address **limitless** connectivity | - Requires a **public IP address for the VPN termination point**   |
| - Used for private, low-latency, limitless bandwidth connectivity               | - Used where encryption and/or transitive routing is needed        |

Transitive routing - when traffic is routed through an intermediate (virtual) network.
In Azure, peer-to-peer transitive routing describes network traffic between two virtual networks that is routed through an intermediate virtual network. I.e. when source and target VNETs are not directly peered with each other.
VNET peering in Azure is a non-transitive relationship between 2 VNETs. In Azure, peer-to-peer transitive routing between two VNETs is routed through an intetmediate VNET with a router.

#### ExpressRoute

ExpressRoute provides more direct and secure connection to Microsoft cloud services. 

Connection between on-premises and Azure via data center partners/NSPs. Secure, private network connectivity.

Allows connect to

- Azure VNETs
- Microsoft services - Microsoft Peeering to connect to Microsoft 365 services (SharePoint Online, Exchange Online)

ExpressRoute VS VPN

| ExpressRoute                                                   | VPN                                                                       |
|----------------------------------------------------------------|---------------------------------------------------------------------------|
| - Provides secure connectivity to **VNETS and Microsoft 365**  | - Provides secure connectivity to **VNETs only**                          |
| - Does not traverse the public inertnet                        | - Traverse the public internet (between the point/site and Azure)         |
| - Does not leverage encryption by default (IPSsec and MACsec)  | - Traffic is encrypted bt default as part of an end-to-end tunnel (IPSec) |
| - Supports up to 10Gbps (**100Gbps** with **ExpressRoute Direct**) | - Supports up to 10Gbps                                                   |

ExpressRoute - extend your on-premises networks to the Microsoft cloud over a private connection with the help of a connectivity provider
ExpressRoute Direct - connect directly to the Microsoft global network. Dedicated dual capacity is available in 10 Gbps and 100 Gbps
Azure ExpressRoute Global Reach - link circuits together for a private connection between your on-premises networks. If you have multiple circuits linking your branch offices to Microsoft, establish Global Reach connections that allow them to exchange data directly.

[ExpressRoute](https://azure.microsoft.com/en-us/services/expressroute/#features) - Use Azure ExpressRoute to create **private connections between Azure datacenters and infrastructure on premises or in a colocation environment**. ExpressRoute connections **don't route through the public internet**, and they **offer more reliability, faster speed, and lower latency** than typical internet connections. In some cases, using ExpressRoute connections to transfer data between on-premises systems and Azure gives you significant cost benefits.

[ExpressRoute Direct](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-erdirect-about) gives you the ability to connect directly into Microsoft’s global network at peering locations strategically distributed around the world. ExpressRoute Direct provides dual 100 Gbps or 10-Gbps connectivity, which supports Active/Active connectivity at scale. You can work with any service provider for ER Direct.

#### Virtual WAN

Azure Virtual WAN helps to automate and optimize connectivity using the hub-and-spoke network architecture/topology. It creates SDN for simplified management on a per region basis.

We have Hub VNET where out resources and controls are centralized at the centet with spoke VNETS connected to it via VNET peering, as well as (possibly) remote users, remote offices connected to it via ExpressRoute/S2S/P2S.

Allows far more simplified and streamlined experience for branch-to-branch, branch-to-Azure, VNET-to-VNET connectivity within our Virtual WAN.

### Designing Networks for Azure Services

VNET-native services
VNET integration
Resource Firewalls

#### VNET-native services

VNET-native services are services which get deployed into VNETs.

Examples:
- Services which gets deployed into VNET always/by default: VMs, AKS, App Service Environments (which is main differece between it and Azure App Services)
- "Public services" which offer an option to deploy directly into VNET: ACI

Not all VNET-native services work in the same way in terms of VNET integration. E.g.:
- ACI can leverage VNET peering but only within the same region
- App Service Environment v2 - had no support for global VNET peering, v3 does

When you need to design solution that is private and accessible from VNET then VNET-native services are easy option to achieve that, otherwise you need to use other VNET integration options.

#### VNET Integration for Services

Services which provide private connectivty from VNET to public resource (i.e. inbound access to resource)

- PrivateLiink (allows the creation of a private endpoint to provide a private IP address to a specific resource which suppors PrivateLink feature)
- Service Endpoints (provide MSFT backbone connectivity from resources in a subnet to a specified **resource provider**/resource type or class)

When outbound access from resource to VNET (e.g. from App Service to VNET) you will need other options which differ for each service:

- For App Service - VNET Integration

VNET integration for App Service

- Provides inbound access to a VNET
- Supported by Standard or Premium tiers
- Supports function apps (not supported with consumption plan)
- Does not support NetBIOS or SMB
- Does not provide inbound app access (**provides outbound access from App Service to VNET**)

Sample use case for App Service VNET integration you have SQL server deployed in VNET and you need your App Service to connecto to this SQL Server.

App Service Networking settings in Azure Portal:

- Inbound Traffic
  - Access restriction
  - App assigned address
  - **Private endpoints**
- Outbound Traffic
  - **VNET integration** - to securely access resources available in or through your Azure VNET.
    - Requires standard or premium app service plan
    - You can simply select VNET in the same region
    - VNET in a different region will require Virtual Network Gateway configured with P2S VPN
  - Hybrid connections - enables your app to access a single TCP endpoint per hybrid connection
    - Supported by the Basic, Standard and premium tier
    - Uses Azure Rely service behing the scenes to create connections with on-premise resources
  - Outbound addressess

#### Resource Firewalls

Many Azure services provide access control through a resource firewall.

- Allows to lock-down inbound access to a (public) resource
- Default Deny once enabled
- Public IP allow rules
- Private VNET allow rules (for that you generally need a service endpoint, so that your traffic goes through Azure backbone and not through the public intetnet)

Access Restricitons Azure UI for App Service

- When no rules added there is only default "Allow All" rule (Any Source Allow with priority 1)
- As soon as you add some other rule default "Allow All" rule goes away, and along with your own rule default "Deny All" rule appears (Any Source Deny, priority 2147483647)

Storage Account > Networking> Firewalls and Virtual Networks

Allowing public IP/CIDR or VNET access with firewall

- Allow access from
  - All networks
  - Selected networks
    - Add existing/new VNET (selecting all or some of subnets, serivce endpoint required to )
    - Firewall
      - Allow public IP or CIDR
- Network routing
  - Microsoft network routing / Internet routing
  - Publish network routing - Y/N
  - Internet routing - Y/N

### Design a Networking Strategy - Requirements/Solutions

- Migrate web app with code which you own to a cloud-based, scalable PaaS service to minimize admin overheads = Azure AppService for hosting
- Provide web app with support of staging updates = Azure AppService deployment slots
- Enable connectivity to Azure hosted web app from on-premise server/equipment via private IP address = 
  - Express Route/private peering to connect on-premises network to VNET (VPN is not an option as traffic goes over public internet and tunnel gets terminated on public IP)
  - Private link/endpoint to provide secure connectivity from VNET to web app

### Design a Networking Strategy Recap

>>>
#### App Service

App Service VNET Integration

- **App Service VNET integration provides connectivity to resources in or through a VNET**;  App Service VNET integration provides **outbound connectivity from the perspective of Azure App Service**. For inbound connectivity to an Azure App Service app, you would require a feature such as Private Link.
- App Service VNet integration requires a supported Standard, Premium, Premium v3, or Elastic Premium App Service pricing tier
- App Service integration is not supported in the Free or Basic App Service Plan tiers

App Service app access control

- Access can be limited by source IP
  - By configuring a new app service access restriction rule
  - By configuring a NSG rule on a subnet

[Access restrictions](https://docs.microsoft.com/en-us/azure/app-service/networking-features#access-restrictions) allow you to build a list of allow and deny rules that are evaluated in priority order. It's similar to the network security group (NSG) feature in Azure networking. You can use this feature in an Azure App Service Environment (ASE).

NSGs provide the ability to control network access within a virtual network. You don't have access to the VMs used to host the App Service Environment itself. They're in a subscription that Microsoft manages. However, you can restrict access to the apps in your App Service Environment (ASE) by setting NSGs on the subnet. [MSFT doccumentation](https://docs.microsoft.com/en-us/azure/app-service/environment/network-info#network-security-groups)

#### Routing

- Default VNET routes allow connectivity between subnets and to the internet

Default VNET routes (lower number got processed first, higher last):
 - **AllovVnetInBound** - 65000, Any Port, Any Protocol, Source VNET, Destination VNET
 - **AllowAzureLoadBalancerInBound** - 65001, Any Port, Any Protocol, Source AzureLoadBalancer, Any Destination
 - **DenyAllInBound** - default DENY rule 65500, Any Port, Any Protocol, Any Source, Any Destination

#### NSGs

- NSGs are stateful, creating an outbound allow rule will automatically allow response traffic; For example, if you create a rule that allows RDP traffic inbound, then you do not need to create a rule that allows the reply traffic back to the originating client.

#### Express Route

- Allows to provide network connectivity between an on-premises network and Azure VNET without using the public internet
- Allows to provide private connectivity to Microsoft services, such as Exchange Online or SharePoint Online; ExpressRoute supports private peering (network-to-VNet connectivity) and Microsoft peering (network-to-Microsoft 365 connectivity), while VPNs can only provide connectivity to an Azure VNet, not to Microsoft 365 services

- ExpressRoute works through network providers that provide connectivity to Azure without using the public internet
- VPNs work over the public internet

#### Service Endpoints

- Provide MSFT backbone connectivity from resources in a subnet to a specified **resource provider**

Service endpoints are configured at the subnet level to a specified resource provider (e.g., Microsoft.Storage). This then establishes a system route for the subnet to the given resource, which ensures traffic from resources in the given subnet will use the Microsoft backbone to reach the given resource type.

#### Private Link

- Allows to provide access to an Azure service using a private IP address
- Allows to provide secure access to a specific resource, instead of an entire resource type

Private Link allows the creation of a private endpoint. The private endpoint is effectively a network interface placed within your selected virtual network with a private IP address. This allows you to create a private IP address that can be used to access an Azure service (or customer-managed solution). Service endpoints do not create private IP addresses for services.

Private Link allows more granular connectivity to be enabled than is possible with service endpoints. For example, with Private Link you can provide a private IP address to a specific storage account in your subscription, whereas with service endpoints, you are enabling a backbone route to all resources of a specific type (i.e., all Microsoft storage accounts that are accessed from the configured subnet).

#### Azure Vitual WAN

- Supports fully meshed, any-to-any hub connectivity
- Simplifies centralized management of a hub and spoke VNET architecture

When using the Azure Virtual WAN Standard SKU, fully meshed hub interconnectivity is supported. This includes branch-to-Azure, branch-to-branch, and VNet-to-VNet connectivity.

Azure Virtual WAN provides a single management pane of glass to help simplify the hub-and-spoke VNet architecture. This includes hub-to-hub interconnectivity across regions.

#### VNET Peering

- VNET peering rpovides private IP address connectivity between VNETs
- Network traffic between VNETs is private and kept on MSFT backbone network
- VNET peering does not provide transitive routing by default

VNet peering leverages the Microsoft backbone to provide low-latency, high-bandwidth interconnectivity between resources on different VNets.

VNet peering helps to provide VNet-to-VNet connectivity so that resources on different VNets can communicate with one another without having to go over the public internet. That is to say that communication is allowed by private IP addresses.

VNet peering does not provide transitive routing. For example, if you have two VNets peered to the same VNet (e.g., SPOKEVNET1 to HUBVNET1, and SPOKEVNET2 to HUBVNET1), this does not allow connectivity between SPOKEVNET1 and SPOKEVNET2 via HUBVNET1. This is possible when using routing, such as an NVA (Network Virtual Appliances) or when configured through Azure Virtual WAN, but it is not supported by default.

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
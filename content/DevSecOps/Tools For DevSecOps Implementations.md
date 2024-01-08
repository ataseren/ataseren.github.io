---
title: Tools For DevSecOps Implementations
tag: cybersecurity, devsecops
---

# DevSecOps Scenarios Tool Notes

- GitGuardian
- Snyk
- Ansible - HashiCorp Vault integration
- Terraform
- Checkov
- Datadog
- PagerDuty
- TheHive

## GitGuardian

GitGuardian is a cybersecurity platform that focuses on detecting sensitive information and data leaks within source code repositories. It specifically aims to identify and prevent the exposure of credentials, API keys, and other confidential information that may be unintentionally included in code repositories. By continuously scanning repositories, GitGuardian helps organizations and developers enhance their security posture by identifying and addressing potential vulnerabilities early in the development process.

GitGuardian can be integrated with a VCS repository to conduct automated secrets detection and remediation. Other than automated scans, GitGuardian gives you the ability to scan the entire commit history, across all branches, of your repositories to check if they are safe. This area of scan is called “perimeter”. Your perimiter is simply anywhere you are storing your shared code repositories. This includes shared repository hosting like GitHub, GitLab, Bitbucket or Azure Repos.

**For AppSec teams,** GitGuardian aims to:**[](https://docs.gitguardian.com/platform/core-concepts/collaboration-between-appsecs-and-devs#for-appsec-teams)**

- Provide AppSec teams with visibility and control over the SCM, DevOps tools and all other components of the SDLC.
- Ease the burden of remediation by pulling developers who own the context closer to the process.

**For development teams,** GitGuardian aims to:**[](https://docs.gitguardian.com/platform/core-concepts/collaboration-between-appsecs-and-devs#for-development-teams)**

- Guide developers throughout the remediation process (feedback collection, steps to follow, etc.) and empower them to resolve incidents by themselves when possible.
- Equip developers with supportive tooling (command-line interface, SDKs, REST API) to implement security guardrails in the environments they are most familiar with.

### **Differences between historical scanning and real-time protection[](https://docs.gitguardian.com/platform/monitor-perimeter/monitored-perimeter#differences-between-historical-scanning-and-real-time-protection)**

**Real-time monitoring[](https://docs.gitguardian.com/platform/monitor-perimeter/monitored-perimeter#real-time-monitoring)**

The first protection and the most effective one for secrets remediation is the **real-time monitoring**.

Real-time monitoring means that every single push event (and its commits) is scanned for secrets as soon as they arrive on your VCS server.

**Historical scanning[](https://docs.gitguardian.com/platform/monitor-perimeter/monitored-perimeter#historical-scanning)**

The second type of protection offered is the ability to **scan the commit history** of all the sources integrated with GitGuardian.

The source criticality feature enables you to assess and assign a level of importance to your monitored sources, helping you prioritize your incidents effectively. This feature allows you to categorize them as low, medium, high, or critical, or leave it unfilled, based on the potential severity of a security incident's impact.

### Secrets Detection

In everyday language, a secret can be any sensitive data that we want to keep private. When discussing secrets in the context of software development, secrets generally refer to digital authentication credentials that grant access to systems or data. These are most commonly API keys, usernames and passwords, or security certificates.

Secrets exist in the context of applications that are no longer standalone monoliths. Applications nowadays rely on thousands of independent building blocks: cloud infrastructure, databases, third-party APIs and services such as Stripe, Slack, HubSpot…

Secrets are so widely used in DevOps environments that there simply can’t be a one-size-fits-all for managing them. There are development secrets used by developers, build secrets, application secrets, infrastructure secrets, etc.

Even for the most mature DevSecOps organizations or teams, secrets management is very difficult to master, because it is a matter of striking a right balance between security and accessibility. This second point is very important for one simple reason: in modern development teams, secrets are required by everyone. Making it hard to use secrets will inevitably lead to the bypassing of the protective layers in place, and lead to practices such as hardcoding them.

In practice, it is easy to see that there is a gap between theory and practice when it comes to handling and sharing credentials in a team, a department, or an organization. For example, the organization may pay for a cloud-based secrets manager, a vault, or maybe even for a dedicated team to administrate these tools, which makes it falsely think it has solved this problem. But under further scrutiny, it would realize that the long-lived credentials are also stored on the devs’ local machines for convenience.

There are various ways with which secrets can be managed in the software development lifecycle:

- Hardcoded in source code and templates (worst way)
- Grouping secrets in common, unencrypted configuration files, such as `.env` (outside of the git repository)
- Encrypting secrets in a GitOps or sealed secrets approach, with decryption key stored in a vault
- Storing secrets in a vault and distributing them through a secrets management service
- Generating dynamic secrets, through a complex secrets management infrastructure
- 

Incidents are open issues that need your attention to be resolved. GitGuardian raises two types of incidents:

- **Secret incidents**
    
    GitGuardian’s secrets detection engine scans source code for hardcoded secrets to display them in dashboard.
    
- **Policy break incidents**
    
    In addition, GitGuardian also checks source code against security policies like the presence of sensitive filenames and file extensions. These policy checks only run during real-time monitoring.
    

A policy is a rule enforced on your perimeter.

Policy break incidents are triggered when an event breaks the policy. Alerts are sent for each event that triggers one or more policy break incidents.

**Filenames policy[](https://docs.gitguardian.com/secrets-detection/detect/secrets-incidents-and-policy-breaks#filenames-policy)**

This policy ensures that files with certain filenames are not committed.

**File extension policy[](https://docs.gitguardian.com/secrets-detection/detect/secrets-incidents-and-policy-breaks#file-extension-policy)**

This policy ensures that files with certain extensions are not committed.

**.gitignore policy[](https://docs.gitguardian.com/secrets-detection/detect/secrets-incidents-and-policy-breaks#gitignore-policy)**

This policy ensures that all your git repositories have a `.gitignore` file in their root directory. This is an indirect security policy as it is the best way to ensure that your secret files are never committed.

A policy break incident is triggered if the file is missing the first time GitGuardian receives an event on the repository or if the file is deleted.

### Honeytokens

**What are honeytokens?[](https://docs.gitguardian.com/honeytoken/core-concepts#what-are-honeytokens)**

GitGuardian Honeytoken allows you to create decoy credentials called “honeytokens” that do not allow any access to any actual customer resources or data. Instead, they act as tripwires that reveal information about the attacker (eg. IP Address, User Agent, Location, etc.).

GitGuardian honeytokens look just like any other secret to attackers. They are designed to be triggered by all types of secret scanners, like open-source projects TruffleHog or Gitleaks, that are often put to wrong use by attackers. This means that if a hacker uses a secret scanner to search for developer secrets, they will trip on the honeytoken, triggering an alert that notifies the security team of a potential security incident.

***To be continued…***

### IaC Security

**[](https://docs.gitguardian.com/iac-security/core-concepts#what-is-infrastructure-as-code)**

Infrastructure as Code (IaC) is the practice of **managing IT infrastructure through machine-readable files** rather than through manual processes. The goal of IaC is to automate infrastructure provisioning and management, saving time and reducing the risk of errors.

IaC provides reproducible, version-controlled, and testable infrastructure, improving reliability and consistency of IT environments.

**[](https://docs.gitguardian.com/iac-security/core-concepts#why-should-you-secure-it)**

As more and more organizations adopt IaC, the need for secure code management becomes increasingly important. Leaving an IaC insecure can result to:

- **Insecure network configuration** by exposing your infrastructure to the internet, making it vulnerable to attack. For example, if an IaC file opens up a port to the public internet, an attacker could exploit that port to gain access to the infrastructure.
- **Insecure storage** by leaving sensitive data exposed, making it vulnerable to theft or attack. For example, if a service stores sensitive data in an unencrypted format, an attacker could easily steal that data.
- **Excessive permissions** by misconfiguring security controls, such as firewalls, intrusion detection systems and access control lists, which could therefore be bypassed by attackers, leaving your whole infrastructure vulnerable.
- **Service disruptions** by causing your services to become unavailable, impacting your customers, business operations or even your brand’s reputation.
- **Compliance violations** by not complying your infrastructure with organisation's security policies or regulatory requirements, leading to fines, penalties, or even legal actions.
- **Configuration drift** by fixing the vulnerabilities directly within the actual infrastructure’s services rather than within the intended IaC files. This leads to leave security gaps and vulnerabilities that attackers can exploit once the IaC is redeployed.

***To be continued…***

## Snyk

Snyk is a platform that allows you to scan, prioritize, and fix security vulnerabilities in your code, open-source dependencies, container images, and infrastructure as code configurations.

Snyk provides visibility in a developer’s workflow and actionable insights. The benefit is engaging developers in security practices as part of their development work. Thus, the focus is on building a secure application rather than overhead-intensive work, such as putting in hard QA gates.

- Snyk Open Source: Snyk Open Source allows you to find and fix vulnerabilities in the open-source libraries used by your applications. You can also find and address licensing issues in or caused by these open-source libraries.
- Snyk Code: SAST tool of Snyk (there is a local version in private beta)
- Snyk IaC:  With Snyk Infrastructure as Code (IaC), you can secure cloud infrastructure configurations before and after deployment. There are two version of Snyk IaC available today:
    
          **Current IaC**: The generally available version of Snyk IaC
    
    **IaC+**: A new version of Snyk IaC that is currently in early access
    
    With both versions of Snyk IaC, you can:
    
    Write secure configurations for HashiCorp Terraform, AWS CloudFormation, Kubernetes, and Azure Resource Manager (ARM) - for IDE, SCM, CLI, and Terraform Cloud/Enterprise workflows.
    
    View issues and receive fix advice so you can make changes directly to code, before applications reach production.
    
    Detect drift and manually created resources in your cloud.
    
    **Onboard, scan, and test deployed cloud environments** for misconfigurations for AWS, Azure, and Google Cloud environments.
    
- Snyk Container: Snyk Container provides tools and integrations to quickly find and fix vulnerabilities. This allows you to create images that have security built-in from the start.
- Snyk Vulnerability Database: The Snyk Vulnerability Database contains a comprehensive list of known security vulnerabilities. This provides the key security information used by Snyk products to find and fix code vulnerabilities.
- Snyk AppRisk: Snyk AppRisk is a product that enables Application Security teams to implement, manage, and scale a modern, high-performing, developer security program.

***See the document for all usage and implementation details.***

## Ansible

Ansible provides open-source automation that reduces complexity and runs everywhere. Using Ansible lets you automate virtually any task. Here are some common use cases for Ansible:

- Eliminate repetition and simplify workflows
- Manage and maintain system configuration
- Continuously deploy complex software
- Perform zero-downtime rolling updates

Ansible uses simple, human-readable scripts called playbooks to automate your tasks. You declare the desired state of a local or remote system in your playbook. Ansible ensures that the system remains in that state.

***See the document for all usage and implementation details.***

## HashiCorp Vault

HashiCorp Vault is an identity-based secrets and encryption management system. It provides encryption services that are gated by authentication and authorization methods to ensure secure, auditable and restricted access to *secrets*. It is used to secure, store and protect secrets and other sensitive data using a UI, CLI, or HTTP API.

A secret is anything that you want to tightly control access to, such as tokens, API keys, passwords, encryption keys or certificates. Vault provides a unified interface to any secret, while providing tight access control and recording a detailed audit log.

API keys for external services, credentials for service-oriented architecture communication, etc. It can be difficult to understand who is accessing which secrets, especially since this can be platform-specific. Adding on key rolling, secure storage, and detailed audit logs is almost impossible without a custom solution. This is where Vault steps in.

Vault validates and authorizes clients (users, machines, apps) before providing them access to secrets or stored sensitive data.

The core Vault workflow consists of four stages:

- **Authenticate:** Authentication in Vault is the process by which a client supplies information that Vault uses to determine if they are who they say they are. Once the client is authenticated against an auth method, a token is generated and associated to a policy.
- **Validation:** Vault validates the client against third-party trusted sources, such as Github, LDAP, AppRole, and more.
- **Authorize**: A client is matched against the Vault security policy. This policy is a set of rules defining which API endpoints a client has access to with its Vault token. Policies provide a declarative way to grant or forbid access to certain paths and operations in Vault.
- **Access**: Vault grants access to secrets, keys, and encryption capabilities by issuing a token based on policies associated with the client’s identity. The client can then use their Vault token for future operations.

### **Why Vault?**

Most enterprises today have credentials sprawled across their organizations. Passwords, API keys, and credentials are stored in plain text, app source code, config files, and other locations. Because these credentials live everywhere, the sprawl can make it difficult and daunting to really know who has access and authorization to what. Having credentials in plain text also increases the potential for malicious attacks, both by internal and external attackers.

Vault was designed with these challenges in mind. Vault takes all of these credentials and centralizes them so that they are defined in one location, which reduces unwanted exposure to credentials. But Vault takes it a few steps further by making sure users, apps, and systems are authenticated and explicitly authorized to access resources, while also providing an audit trail that captures and preserves a history of clients' actions.

The key features of Vault are:

- **Secure Secret Storage**: Arbitrary key/value secrets can be stored in Vault. Vault encrypts these secrets prior to writing them to persistent storage, so gaining access to the raw storage isn't enough to access your secrets. Vault can write to disk, [Consul](https://www.consul.io/), and more.
- **Dynamic Secrets**: Vault can generate secrets on-demand for some systems, such as AWS or SQL databases. For example, when an application needs to access an S3 bucket, it asks Vault for credentials, and Vault will generate an AWS keypair with valid permissions on demand. After creating these dynamic secrets, Vault will also automatically revoke them after the lease is up.
- **Data Encryption**: Vault can encrypt and decrypt data without storing it. This allows security teams to define encryption parameters and developers to store encrypted data in a location such as a SQL database without having to design their own encryption methods.
- **Leasing and Renewal**: All secrets in Vault have a *lease* associated with them. At the end of the lease, Vault will automatically revoke that secret. Clients are able to renew leases via built-in renew APIs.
- **Revocation**: Vault has built-in support for secret revocation. Vault can revoke not only single secrets, but a tree of secrets, for example all secrets read by a specific user, or all secrets of a particular type. Revocation assists in key rolling as well as locking down systems in the case of an intrusion.

## Ansible - Hashicorp Vault Integration

[https://docs.ansible.com/ansible/latest/collections/community/hashi_vault/index.html](https://docs.ansible.com/ansible/latest/collections/community/hashi_vault/index.html)

[https://docs.ansible.com/ansible/latest/collections/community/hashi_vault/hashi_vault_lookup.html#ansible-collections-community-hashi-vault-hashi-vault-lookup](https://docs.ansible.com/ansible/latest/collections/community/hashi_vault/hashi_vault_lookup.html#ansible-collections-community-hashi-vault-hashi-vault-lookup)

## Terraform

HashiCorp Terraform is an infrastructure as code tool that lets you define both cloud and on-prem resources in human-readable configuration files that you can version, reuse, and share. You can then use a consistent workflow to provision and manage all of your infrastructure throughout its lifecycle. Terraform can manage low-level components like compute, storage, and networking resources, as well as high-level components like DNS entries and SaaS features.

The core Terraform workflow consists of three stages:

- **Write:** You define resources, which may be across multiple cloud providers and services. For example, you might create a configuration to deploy an application on virtual machines in a Virtual Private Cloud (VPC) network with security groups and a load balancer.
- **Plan:** Terraform creates an execution plan describing the infrastructure it will create, update, or destroy based on the existing infrastructure and your configuration.
- **Apply:** On approval, Terraform performs the proposed operations in the correct order, respecting any resource dependencies. For example, if you update the properties of a VPC and change the number of virtual machines in that VPC, Terraform will recreate the VPC before scaling the virtual machines.

[https://developer.hashicorp.com/terraform/intro/core-workflow](https://developer.hashicorp.com/terraform/intro/core-workflow)

## Checkov

Checkov is a static code analysis tool for scanning infrastructure as code (IaC) files for misconfigurations that may lead to security or compliance problems. Checkov includes more than 750 predefined policies to check for common misconfiguration issues. Checkov also supports the creation and contribution of custom policies.

Checkov scans these IaC file types:

- Terraform (for AWS, GCP, Azure and OCI)
- CloudFormation (including AWS SAM)
- Azure Resource Manager (ARM)
- Serverless framework
- Helm charts
- Kubernetes
- Docker

Custom policies can be created to check cloud resources based on configuration attributes (in [Python](https://www.checkov.io/3.Custom%20Policies/Python%20Custom%20Policies.html) or [YAML](https://www.checkov.io/3.Custom%20Policies/YAML%20Custom%20Policies.html) or connection states (in [YAML](https://www.checkov.io/3.Custom%20Policies/YAML%20Custom%20Policies.html)). For composite policies, Checkov creates a cloud resource connection graph for deep misconfiguration analysis across resource relationships.

With Checkov you can scan a repository, branch, folder, or a single file with attribute-based misconfigurations or connection state errors. When running Checkov, you can also:

- Review scan results
- Suppress or skip
- Scan credentials and secrets
- Scan Kubernetes clusters
- Scan Terraform plan output and 3rd party modules

In addition to integrating with your code repository, Checkov can also integrate with your automated build pipeline via CI/CD providers. When your build tests run, Checkov will scan your infrastructure as code files for misconfigurations and you can review the output directly in your CI pipeline.

## Datadog

Datadog is a tool that allows you to monitor cloud infrastructure, Windows and Linux hosts, system processes, serverless functions, and cloud-based applications. It can be used to visualize data, explore metrics, manage logs, and perform various other tasks.

Advertisement

### What are the main use cases for Datadog?

Datadog allows you to collect metrics and gather real-time in-depth insights about your IT infrastructure. Here are the main use cases for the app:

- IT pros can create, edit, and manage alerts and notifications about their IT infrastructure.
- Organizations can use [Application Performance Monitoring](https://www.datadoghq.com/product/apm/) (APM) to reduce latency and eliminate errors
- They can test production environments and performance.
- They can set up multiple integrations that gather metrics, traces, and logs to send data to the platform.
- They can use it as a security platform to detect threats and misconfiguration of applications in their infrastructure.
- If you use Jenkins, which is an automation server for deploying software, the app can help to visualize Jenkins job metrics and pipeline execution.

## Terraform - Datadog Integration (or other monitoring tools)

[https://www.datadoghq.com/blog/managing-datadog-with-terraform/](https://www.datadoghq.com/blog/managing-datadog-with-terraform/)

## TheHive

TheHive is a scalable Security Incident Response Platform, tightly integrated with MISP (Malware Information Sharing Platform), designed to make life easier for SOCs, CSIRTs, CERTs and any information security practitioner dealing with security incidents that need to be investigated and acted upon swiftly.
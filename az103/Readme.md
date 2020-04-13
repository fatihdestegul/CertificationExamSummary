# Summary

## 06 Import and Export Data to Azure

- Import/Export service is accessible via Portal
    - Used to track data import (upload) jobs, export (download) jobs
- Import/Export command-line tool
    - Preparing disk drives that are shipped
    - Copying data your drive
    - Encrypts data with BitLocker
- Disk Drives
    - HDDs, SSDs
    - Import jobs: You ship drives containing your data
    - Export jobs: You ship empty drives
- Import job workflow
    - Creating --> Shipping --> Transferring --> Packaging --> Complete
- Akamai, Verizon, Verizon Premium (CDN providers)
- Choose Akamai over Verizon if you need media optimized CDN
- Choose Verizon over Akamai if you need to manage rules, re-directions, blocking contents

## 07 Configure Azure Files

- Read Access GEO Redundant Storage (RA -GRS) will create 6 copy of data in another region
- Azure Files --> Connect
    - Pop-ups Powershell / Linux script to connect File Share
- Azure File Sync
    - Used to sync your drives in data center with Azure Files
    - It is not avaiable in all regions
    - File Share and File Sync should be in the same region
    - Storage account should have a File Share to use this service
    - RDP to VM -->  turn-off IE enhanced security Configuration --> copy & paste & run given script which downloads File Share Sync agent onto VM

## 08 Implement Azure Backup

- Microsoft Azure Recovery Services (MARS)
    - On premises or Azure Backup
    - Recorvery Vault --> Backup 
    - Vault secret is valid for 2 days
- Business Continuity Strategies
    - High Availability - Run another instance of apps in case of catastrophic failure
    - Disaster Recovery - Run apps in secondary datacenter if a failure occurs
    - Backup - Restore your data
        - Backup solution purpose built for Cloud
        - Unlimited scaling
        - Unlimited Data Transfer
        - Multiple Storage Options 
            - Locally Redundant Storage (LRS) replicates 3 times in a storage scale unit in a datacenter
            - Geo Redundant Storage (GRS) replicates in a secondary region.. Cost more than LRS but recommended option
        - Long Term Retention
        - Application-Consistent Backups
        - Data Encryption
- Azure Backup Components
    - Azure Backup (MARS) Agent (Windows only)
    - System Center DPM
    - Azure Backup Server
    - Azure IaasVM Backup
- Other Recovery Options (More manual recovery types)
    - Snapshot Recovery
    - Geo-Replication
    
## 09 Create and Configure a VM for Windows and Linux

- VM Types
    - A - Basic: Basic version of the A series for testing and development
    - A - Standard: General-purpose VMs.
    - B - Burstable: Burstable instances that can burst to the full capacity of the CPU when needed.
    - D - General Purpose: Built for enterprise applications. DS instances offer premium storage.
    - E - Memory Optimized: High memory-to-CPU core ratio. ES instances offer premium storage.
    - F - CPU Optimized: High CPU core-to-memory ratio. FS instances offer premium storage.
    - G - Godzilla: Very large instances ideal for large databases and big data use cases.
    - H - High performance compute: High performance compute instances aimed at very high-end computational needs such as molecular modelling and other scientific applications.
    - L - Storage optimized: Storage optimized instances which offer a higher disk throughput and IO.
    - M - Large memory: Another large-scale memory option that allows for up to 3.5 TB of RAM.
    - N - GPU enabled: GPU-enabled instances
    - SAP HANA on Azure Certified Instances: Specialized instances purposely built and certified for running SAP HANA.
- VM Specializations
    - S: Premium Storage options available eg. DSv2
    - M: Larger memory configuration of instance type eg. Standard A2m_v2
    - R: Supports remote direct memory access (RDMA)

- Azure Compute Units (ACUs)
    - Way to compare CPU performance between different types/sizes of VM
    - Microsoft created performance benchmark
    - A VM with an ACU of 200 has twice the performance of a VM with an ACU of 100
    - https://docs.microsoft.com/en-us/azure/virtual-machines/windows/
    - https://docs.microsoft.com/en-us/azure/virtual-machines/acu?toc=/azure/virtual-machines/windows/toc.json&bc=/azure/virtual-machines/windows/breadcrumb/toc.json

- WinRM Self-Signed Certificate Procedures
    - Create a Key Vault
    - Create a self-signed certificate 
    - Upload certificate to the Key Vault
    - Get the URL for your certificate from the Key Vault
    - Reference your self-signed certificate when you create the VM


## 10 Automate Deployment of VMs

- VM Images
    - Custom Images
        - Do-it-yourself image
        - Windows - Sysprep
        - Linux - sudo waagent - deprovision + user
        - Generalize in Azure
        - Create image
    - Marketplace Images
        - Provided for you in the Azure Marketplace
        - Properties
            - Publisher
            - Offer
            - SKU
- Create a Custom Image
    - Connect to VM --> Run sysprep under system32
    - Sysprep: To deploy a Windows image to different PCs, you have to first generalize the image to remove computer-specific information such as installed drivers and the computer security identifier (SID).
    - VM would be stopped. --> Capture --> It will create VM Image

## 11 Manage Azure VM Storage and Networking

- VM Storage Types
    - Standard Storage
        - Backed by Traditional HDD
        - Most cost effective
        - Max throughput - 60 MS/S per disk
        - Max IOPS - 500 IOPS per disk
    - Premium Storage
        - Backed by SSD drives
        - Higher performance
        - Max throughout - 250 MB/S per disk
        - Max IOPS - 7500 IOPS per disk

- Managed vs Unmanaged Disks
    - Unmanaged Disks
        - DIY option (Do it yourself)
        - Management overhead (20000 IOPS per storage account limit)
        - Supports all replication modes (LRS, ZRS, GRS, RA-GRS)
            - LRS: Logically Replicated Storage
                - Replicated three times withing a storage scale unit hosted in a data center in the same region as your storage account was created.
            - ZRS: Zone Replicated Storage
                - Replicated three times across one or two datacenters in addition to storing three replicas similar to LRS. Data stored in ZRS is durable even in the event that primary datacenter in unavailable or unrecoverable.
            - GRS: Geographically Replicated Storage
                - Replicates your data to a second region that is hunderds of miles away from the primary region. Your data is curable even in the event of a complete region outage.
            - RA-GRS: Read Only Geographically Replicated Storage
                - Same replication as per GRS but also provides read access to the data in the other region.
    - Managed Disks
        - Simplest option
        - Lower management overhead as Azure manages storage accounts

- Data is replicated across multiple datacenters?
    - LRS: No  ZRS: Yes GRS: Yes RA-GRS: Yes
- Data can be read from a secondary location and the primary location?
    - LRS: No  ZRS: No GRS: No RA-GRS: Yes
- Number of copies of data maintained on separate nodes?
    - LRS: 3  ZRS: 3 GRS: 6 RA-GRS: 6

- Disk Caching
    - Method for improving performance of VHDs (Virtual Hard Disk)
    - Utilizes local RAM and SSD drives on underlying VM host

    - Default and Allowed settings
                    Default Cache Setting     Allowed Settings
        OS disk:     Read-Write                Read-Only or Read-Write
        Data disk:    None                    None, Read-only or Read-Write

    - Read-only caching: Improve latency and potentially gain higher IOPS per disk
    - Read-Write caching: Ensure you have a proper way to write data from cache to persistent disks

## 12 VM Availability

Azure does VM Availability with Availability Sets

- Potential for VM impact
    - Planned Maintenance
    - Unplanned Hardware Maintenance
    - Unexpected Downtime
- Availability Sets
    - Group two or more machines in a set
    - Separated based on Fault Domains and Update Domains

- Availability Zones
    - Offer 99.99% availability
    - Minimize impact of planned and unplanned downtime
    - Enforce them like Availability Sets, but now you choose your specific zone in Azure

## 13 VM Scale Sets

VM Scale Sets Overview
    - Set minimum and maximum instance counts
    - Scale out based on a variety of metrics - infrastructure or application
    - Scale out based on a schedule (Black Friday data etc.)
    - Remember to account for sessions when scaling in on web servers. If you got a lot of users, scaling in won't take care of sessions. You need to think of it. e.g. Redis Cache..

## 14 Azure Networking

https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-faq

- VNET
    - Isolation
    - Internet Access
    - Azure Resources (VMs and Cloud Services)
    - VNet Connectivity
    - On-Premises Connectivity
    - Traffic Filter
    - Routing
- VNETs: Key Points
    - Primary building block for Azure networking
    - Private network in Azure based on an address space prefix
    - Create subnets in your VNet with your IP ranges
    - Bring your own DNS or use Azure-provided DNS
    - Choose yo connect the network to on-premises or the internet

- IP Addressing
    - DHCP - Azure-provided/managed service
    - All addresses are DHCP-based
    - Addresses are not allocated until Azure object is created
    - Addresses are recovered when object is deallocated
    - Static addresses are the equivalent DHCP reservations
    - Address prefix comes from VNet/subnet definitions

Are there any restrictions on using IP addresses within these subnets?
Yes. Azure reserves 5 IP addresses within each subnet. These are x.x.x.0-x.x.x.3 and the last address of the subnet. x.x.x.1-x.x.x.3 is reserved in each subnet for Azure services.

    x.x.x.0: Network address
    x.x.x.1: Reserved by Azure for the default gateway
    x.x.x.2, x.x.x.3: Reserved by Azure to map the Azure DNS IPs to the VNet space
    x.x.x.255: Network broadcast address

## 15 Create Connectivity between Virtual Networks

- S2S (Site-to-Site) (Extend your network to Cloud)
    Connect on premises data center to Azure
    Requires a VPN device in a enterprise datacenter that has a public IP address assigned to it
    Must not be located behind a NAT
    S2S connections can be sued for cross-premises and hybrid configurations

- P2S (Connecting clients directly to network)
    - Secure connection from an individual computer. Great for remote worker situations.
    - No need for a VPN device or public IP. Connect whereverr user has internet connection.
    - Throughput up to 100 Mbps (unpredictable due to internet)
    - Doesn't scale easily, so only useful for a few workstations

- Multi-Site
    The same with S2S for more sites
    Different offices are connected to the same VPN Gateway

- ExpressRoute
    Privacy of data. More reliable higher throughput..Good bandwidth
    Layer 3 Connectivity
    Connectivity in all regions
    Global connectivity
    Dynamic Routing
    Built-In Redundancy
    It can take months to setup.. It has a workflow
    Unlimited vs Metered. Metered has restrictions on outbound limits

- Hybrid Connection
    Allows Web App to talk to the data center
    Hybrid Connection can be shared across Web Apps and Mobile Apps
    All Web App Frameworks supported
    It has a connection manager can be installed >= Windows2008 Server
    e.g. You have a SQL server in your network and a cloud app on Azure..

## 16 Configure Name Resolution

- Internet Access
    - All resources in a VNet can communicate to the internet by default
    - Private IP is SNAT (Source NAT) to a public IP selected by Azure
    - Outbound connectivity can be restricted via routes or traffic filtering (NSG etc..)
    - Inbound connectivity without SNAT requires public IP

- DNS in Azure
    - Azure provided DNS
    - Customer DNS Server
        - IaaS Server with DNS
        - Infoblox Virtual Appliance

- DNS Scenarios and Recommendations
    - Name resolution between role instances or virtual machines in the same virtual network --> Choose Azure provided DNS
    - Name resolution between role instances or virtual machines in the different virtual network --> Choose Customer-managed DNS Servers
    - Resolution of on-premises computers and service names from role instances or virtual machines in Azure --> Choose Customer-managed DNS Servers
    - Resolution of Azure hostnames from on-premises computers --> Choose Customer-managed DNS Servers

- VNET --> DNS Servers --> Custom --> Add DNS Servers --> Save
    Note: VMs will require restart to utilize updated settings!

## 17 Create and Configure a Network Security Group (NSG)

- Is a Network Filter
- Used to allow or restrict traffic to resources in your Azure network
- Inbound / Outbound rules
- Associated to subnet or NIC (and individual VMS in classic)

- NSG Rule Priority
    - Rules are enforced based on priority
    - Range from 100 to 4096
    - Lower numbers have higher priority

- NSG Default Tags
    - System Provided to identify Groups of IP Address
    - Virtual Network
    - Azure Load Balancer
    - Internet

## 18 Load Balancing

- Load Balancer (Basic and Standard)
    Basic
    - Layer 4
    - Supports up to 100 instances
    - Service monitoring (probes..)
    - Automated reconfiguration
    - Hash-based distribution (5 tuple hash, souce ip source port destination ip dest port ..)
    - Internal and public options
    - Open by default
    Standard
    - Layer 4
    - Supports up to 1000 instances
    - Any virtual machine in a single VNET. (In Basic you are tight to a availability set)
    - HTTPS support
    - Availability Zone support
    - Secure by default

Exam Tip: While the Basic Load Balancer is scoped to an availability set, the Standard Load Balancer is scoped to the entire virtual network.
- Application Gateway
    - Layer 7 application load balancing
    - Cookie-based session affinity
    - SSL offload
    - End-to-end SSL
    - Web application firewall (SQL injections, cross-site scripting)
    - URL-based content routing
    - Requires its own subnet
    - Highly Available
- Traffic Manager
    - DNS-level
    - Any application protocols supported
    - Only supports Internet facing applications
    - Monitoring supported via Http(s) GET.

## 19 Network Monitoring

- Extensions
    - Network Watcher
 
## 20 Manage Azure Active Directory

Azure AD (AAD)
    - Modern AD service build directly for the cloud
    - Often the same as O365 directory service
    - Can sync with On-premises directory service
    - Enterprise identity solution, Single Sign-on, MFA, Self Service
Active Directory Domain Services (ADDS)
    - Legacy Active Directoru since Windows 2000
    - Traditional Kerberos and LDAP functionality
    - Deployed on Windows OS usually on VMs
Azure Active Directory Domain Services (AADDS)
    - Provides managed domain services
    - Allows you to consume domain services without the need to path and maintain domain controllers on IaaS
    - Domain Join, Group Policy, LDAP, Kerberos, NTLM; all supported

- Free Active Directory
    - Has 500K Object limit
    - Allows 10 apps
    - Doesn't give group based access 

- P2 Active Directory
    - Identity Protection
    - Privileged Identity Management (People can have different roles for certain tasks and change to default afterwards.)

## 21 Implement and Manage Hybrid Identities

AD Connect Components
- Synchronization Services
    - Filtering, which object do you want to sync?
    - Password hash syncronization, end-user can use the same password with on-premises AD
        - Passwod Sync - Ensures user passwords are the same in both directories (AD DS and Azure AD)
        - Passthrough Authentication - Easy method to keep users and passwords aligned. When a user logs into Azure AD, the request is forwarded to AD DS. Essentially, a single source.
        - AD FS - Use AD Fedaration Services server to fully federate across AD DS and Azure AD, along with other services.
    - Password writeback, you can change password on Azure and can write back to on-premises
    - Device writeback
    - Prevent accidental deletes
    - Automatic upgrade

- AD Fedaration Services (optional)
- Health Monitoring

- Single Sign On 
    - Provided by Azure AD Connect for users using password sync or passthrough authentication
    - Company device with modern browser required
    - User not required to authenticate with Azure AD if they are logged on with their AD DS credentials
- Multifactor Authentication (MFA)
    - Works by requiring 2 or more of the following verification methods:
        - Something  you know (Password)
        - Something you have (e.g. cellphone)
        - Something you are (Biometrics)

Azure AD B2C
- Cloud Identity Solution for Web and Mobile Apps 
- Highly scalable to hundreds of millions of identities
- Enables authentication for:
    - Social Accounts
    - Enterprise Accounts

Azure AD B2B
- Allows you to collabrate with partners outside of your organization
- Users receive an email with a confirmation link upon invitation
- Imported users are "Azure AD External User Objects"
- Access to shared apps, resources, documents etc.
- Partners access with their own credentials


## 22 Identity Design

Design Authentication
- Cloud Authentication
    - Cloud-only
    - Password Hash Sync + Seamless SSO
    - Pass-Through Authentication + Seamless SSO
- Federated Authentication
    - AD FS
    - 3rd Party Federation Providers

If you need identity protection, you need Password Hash Sync
If you have a certain customizable requirement are not natively supported, you should consider Federation
If you want to enforce on-premises policies that you have in your Active Directory and ain't Azure AD, you probably should go with Pass-through Authentication and then you can use those on-premises policies. Ideally, you can use Pass-through Auth + Seamless SSO but this is an important decision that will hard to change later.

## 23 Azure Resource Manager (ARM)

- Apply Infrastructure as Code
- Download templates from Azure Portal
- Author new templates
- Use Quickstart templates, provided by Microsoft

ARM File Types
    - ARM Template file (Describe the configuration of your infra via a JSON file)
    - ARM Template Parameter File (Separate your parameters (optional))
    - Deployment Scripts (e.g. Powershell for Deployment)
ARM Template Constructs
    - Parameters (Define fhe inputs you want to pass into the ARM template during deployment)
    - Variables (Values that you can use throughout your template. Used to simplify your template by creating reuse of values)
    - Resources (Define the resources you with to deploy or update)
    - Outputs (Specify values that are returned after the ARM deployment is completed.)

Linking Templates
Inline 
    - Create entire ARM template in body of existing template
External
    - Link to an external template with an INLINE or EXTERNAL parameter set

Key ARM Functions
Copy --> Enable declare resource and you can say "hey want 3 more times"
copyIndex() --> We wanna name something and we wanna name something in the iteration
dependsOn --> A specific variable depends on another variable

## 24 Azure Automation and Automation DSC

Implement Runbooks with Azure Automation
    - Runbooks, Runbooks Gallery
    - Python, Powershell
DSC : Desired State Configuration
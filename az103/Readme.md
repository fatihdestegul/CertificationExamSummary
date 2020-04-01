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

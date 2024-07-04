
# Infrastructure Modernization: From On-prem to Azure using Azure Backup Solution

This repository provides a step-by-step guide to modernizing a corporate datacenter by migrating on-premises infrastructure to Azure. The focus is on setting up a shared file system using Azure Files, connecting it to two virtual machines (VMs), and implementing a robust backup and restore solution using Azure Backup and Recovery Services. This modernization effort replaces the old-fashioned tape backup system with a more efficient and reliable Azure-based solution.

## Project Overview

In this project, we will create a shared file system on Azure Storage, mount it on two VMs, and configure backup and restore operations using Azure Backup Service and Azure Recovery Services. This ensures that in case files are accidentally deleted, they can be easily restored from snapshots or backups.

## Steps

1. **Create Two Virtual Machines**
   - Set up two VMs that will act as application servers.

2. **Create Storage Account and File Share**
   - **Storage Account Name**: (Give it a unique name)
   - **File Share Name**: `app-shared` (Within Storage account console: `Data storage > File shares > New file share`)
   - **Quota**: 1 GB (Edit quota to change size)
   - **Connect VMs to File Share**: Follow the Linux connection instructions and paste the code into both GitBash sessions after SSH into the VMs.
     
![https://i.imgur.com/UOtQHQ8.png](https://i.imgur.com/UOtQHQ8.png)

   - **Verify Directories inside one of VMs**:
     ```bash
     df -kh
     cd /mnt/app-shared
     ```
     
![https://i.imgur.com/UbwBK4E.png](https://i.imgur.com/UbwBK4E.png)

   - **Create Sample Files**:
     ```bash
     echo 'Contents of file 1' >> /mnt/app-shared/file1.txt
     echo 'Contents of file 2' >> /mnt/app-shared/file2.txt
     echo 'Contents of file 3' >> /mnt/app-shared/file3.txt
     echo 'Contents of file 4' >> /mnt/app-shared/file4.txt
     echo 'Contents of file 5' >> /mnt/app-shared/file5.txt
     ```

![https://i.imgur.com/J6L9eEf.png](https://i.imgur.com/J6L9eEf.png)

3. **Configure Backup**
   - Navigate to `Data storage > File shares > app-shared`.
   - Verify the presence of the 5 text files.
   - Go to `Operations > Backup` and create a new backup with the default vault name, resource group `azurebootcamp`, and default backup policy.
   - Enable backup.

![https://i.imgur.com/It1zsjZ.png](https://i.imgur.com/It1zsjZ.png)
![https://i.imgur.com/j2q1nwZ.png](https://i.imgur.com/j2q1nwZ.png)

4. **Monitor Backup Jobs**
   - Navigate to `Recovery Services vaults` and select the vault.
   - Under `Monitoring > Backup jobs`, wait for the status to show as completed.

![https://i.imgur.com/Allcs9n.png](https://i.imgur.com/Allcs9n.png)

5. **Perform Backup and Snapshots**
   - Go back to the storage account.
   - Navigate to `Data storage > File shares`.
   - Under `Operations > Backup`, select `Backup now` and confirm.
   - In `Recovery Services vaults`, monitor the backup jobs and confirm completion.
  
![https://i.imgur.com/TxdtwlG.png](https://i.imgur.com/TxdtwlG.png)
![https://i.imgur.com/MXRJtdE.png](https://i.imgur.com/MXRJtdE.png)

6. **Delete and Restore Files**
   - On one of the VMs, delete two files:
     ```bash
     rm -rf /mnt/app-shared/file2.txt
     rm -rf /mnt/app-shared/file5.txt
     ls -ltr
     ```
     
![https://i.imgur.com/xem5tKQ.png](https://i.imgur.com/xem5tKQ.png)

   - Navigate to `app-shared` in the Azure portal.
   - Go to `Operations > Backup > File Recovery`.
   - Select the snapshot and restore the deleted files to their original location, skipping conflicts.

![https://i.imgur.com/eWkua3V.png](https://i.imgur.com/eWkua3V.png)
![https://i.imgur.com/vD5qB6x.png](https://i.imgur.com/vD5qB6x.png)
![https://i.imgur.com/a3nA25I.png](https://i.imgur.com/a3nA25I.png)
![https://i.imgur.com/QoVxQbU.png](https://i.imgur.com/QoVxQbU.png)

6. **Verify Restore and Finalize**
   - In `Recovery Services vault`, refresh backup jobs and view details of the restore.
   - Verify that the deleted files have been restored in the file share.

![https://i.imgur.com/otWp57z.png](https://i.imgur.com/otWp57z.png)

7. **Cleanup**
   - Delete the storage account.
   - Navigate to `Settings > Locks` and remove the lock if any.

## Things I Learned

1. **Azure File Shares**: Setting up and managing shared file systems on Azure.
2. **VM Configuration**: Connecting VMs to a shared file system using Azure Files.
3. **Backup and Recovery**: Implementing and managing backups using Azure Backup Service and Recovery Services.
4. **Snapshot Management**: Creating and using snapshots for file recovery.
5. **Resource Management**: Efficiently organizing and managing resources within Azure.

By following these steps, you will modernize your infrastructure by moving from on-premises to Azure, ensuring secure and reliable backup and restore operations for your shared file system.

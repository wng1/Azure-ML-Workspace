# MongoDB to Cosmos DB Migration Guide

This guide provides a step-by-step process for migrating data from a MongoDB instance to a Cosmos DB account on Azure. It covers the environment setup, configuring source and target databases, using the Azure Database Migration Service (DMS) for data migration, and verifying the migration.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Prepare Your Azure Environment](#prepare-your-azure-environment)
- [Set Up the MongoDB Instance (Source)](#set-up-the-mongodb-instance-source)
- [Create a Cosmos DB Account (Target)](#create-a-cosmos-db-account-target)
- [Use Azure Database Migration Service (DMS)](#use-azure-database-migration-service-dms)
- [Post-Migration Verification and Testing](#post-migration-verification-and-testing)
- [Finalize and Monitor](#finalize-and-monitor)
- [Conclusion](#conclusion)

---

## Prerequisites

- An active Azure account.
- Basic knowledge of MongoDB and Cosmos DB.
- Familiarity with the Azure Portal and Azure Cloud Shell (or an SSH client).

---

## Prepare Your Azure Environment

1. **Log into Your Azure Account:**  
   Sign in to your Microsoft Azure account.

2. **Create or Verify a Resource Group:**  
   Either use an existing resource group or create a new one dedicated to the migration project.

3. **Establish a Virtual Network (VNet):**  
   - Create a Virtual Network from the Azure Marketplace.
   - Configure Network Security Groups (NSGs) to allow only trusted IP addresses and required ports.
   - **Note:** Instead of disabling Azure DDoS protection or the firewall, adjust your NSG rules to ensure secure, limited access.

---

## Set Up the MongoDB Instance (Source)

1. **Deploy a MongoDB Server:**  
   - For a test or staging environment, deploy an Ubuntu server (e.g., Ubuntu 18.04 LTS) from the Azure Marketplace.
   - Attach the server to the previously created VNet.

2. **Configure Security on the MongoDB Server:**  
   - Set inbound security rules (using NSGs) to open only the required ports (commonly `27017` or your chosen port).
   - Install MongoDB and configure it securely by:
     - Installing the necessary packages.
     - Setting up the MongoDB configuration file.
     - Creating an administrative user with strong credentials.
     - Restricting access to only trusted hosts.

3. **Verify MongoDB Operation:**  
   - Connect to the server via Azure Cloud Shell or your preferred SSH client.
   - Test the MongoDB setup by connecting locally (e.g., using `mongo --host localhost --port <your_port>`), logging in with admin credentials, and selecting the target database (e.g., `use Database1`).

---

## Create a Cosmos DB Account (Target)

1. **Deploy Cosmos DB with the MongoDB API:**  
   - In the Azure Portal, create a new Cosmos DB account.
   - Choose the **MongoDB API** to ensure compatibility with your existing MongoDB-based application.
   - Configure additional settings such as throughput and region based on your workload.

---

## Use Azure Database Migration Service (DMS)

1. **Set Up DMS:**  
   - Search for and create an Azure Database Migration Service resource.
   - Follow the wizard to create a new migration project.

2. **Configure Migration Endpoints:**  
   - **Source Endpoint:** Provide the MongoDB server details (IP address, port, and credentials).
   - **Target Endpoint:** Provide the Cosmos DB account details (using the MongoDB API connection string).

3. **Select Migration Mode:**  
   - Choose between an offline (one-time) migration or an online (continuous synchronization until cutover) migration based on your requirements.

4. **Run the Migration Wizard:**  
   - Validate connectivity between the source and target.
   - Start the migration process.
   - Monitor the progress and wait for confirmation that the migration is complete.

---

## Post-Migration Verification and Testing

1. **Validate Data Integrity:**  
   - Use commands such as `db.CollectionName.count()` on both the MongoDB source and the Cosmos DB target to verify document counts.
   - Perform additional checks (e.g., verifying sample documents and indexes) to ensure data integrity.

2. **Application Testing:**  
   - Update your application's connection string to point to the new Cosmos DB instance.
   - Test the application functionality thoroughly to confirm that it works as expected with Cosmos DB.

---

## Finalize and Monitor

1. **Secure and Optimize:**  
   - Double-check all NSG and firewall rules to ensure continued secure access.
   - Optimize Cosmos DB settings (such as throughput and scaling) according to your application's needs.

2. **Documentation and Monitoring:**  
   - Document the entire migration process and any configuration changes.
   - Set up monitoring (using Azure Monitor and Cosmos DB metrics) to track performance and security.

---

## Conclusion

In summary, you can migrate your MongoDB data to Cosmos DB on Azure securely and efficiently, while ensuring data integrity and minimal downtime. 

Feel free to contribute or suggest improvements via pull requests or issues on GitHub!

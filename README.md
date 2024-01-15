# Azure data base migration
The description of an implementaion of a database migration using the various tools and sotwares provided by the Microsoft [Azure cloud computing services](https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize?client_id=8e0e8db5-b713-4e91-98e6-470fed0aa4c2&response_type=code%20id_token&scope=openid%20profile&state=OpenIdConnect.AuthenticationProperties%3DjZEd0hq1c0oKq_PnKRqosFjsPt1YLhnhbcLnVWsmz_wuJhb4z6iRBjsenupkL0o2scLAQMHEbpfhlY_R23zsvheNvZWuKKWEFPRnC_kQDtWKc2PVaVIYexSeyWNrE-VNjaT2oEQQH_K2HtMQdX__-ySAInJpHBKHTM-ypIkDDVUtbQzuVng2lMq7wfhB7SeNJgbI0h93fLphQG5oXQtKS3LPvb7luxrEJw2yy2PIiPvI8AmpaV4IFYnSZOOyRJdrcB9XIBGDp4jJ5ZGfyxcSO9CPT786hzj8xOZLnXvNIgSSM2uPS2dksDPHyGAxjjFkX862Ys6dWwPIh_T7LXkHJiUgi9mo9lS8YrnoKKULgpJST9gMqmde2So0xZkaHuuNb_cpeHaARt0ZLyLy_vIh2jFzsYCjUUVALc-hiRI0-LrAJuqOu78xc9BSQwRt_9GEpS_96vJckfRnTwolCFHlDcAd-zoR177TUOOVYEcCZCY&response_mode=form_post&nonce=638407554444943112.NTBiNmM4MmYtNWFiMS00MzAwLWI2MTEtNjM5MDExMTI2NGFjZWY0ZTE0YmItMDAxMy00MjUzLTk5NTItY2RkZWZlZDg1N2Jl&redirect_uri=https%3A%2F%2Fsignup.azure.com%2Fapi%2Fuser%2Flogin&max_age=86400&post_logout_redirect_uri=https%3A%2F%2Fsignup.azure.com%2Fsignup%3Foffer%3Dms-azr-0044p%26appId%3D102%26ref%3D%26redirectURL%3Dhttps%3A%2F%2Fazure.microsoft.com%2Fget-started%2Fwelcome-to-azure%2F%26l%3Den-us%26srcurl%3Dhttps%3A%2F%2Fazure.microsoft.com%2Ffree&x-client-SKU=ID_NET472&x-client-ver=6.34.0.0)

Such tools provide end users ease and advanced tooling and is a major point of the following description.

<h1> Table of Contents</h1>

- [Production enviroment set up](#production-enviroment-set-up)
- [Migrate to Azure SQL database](#migrate-to-azure-sql-database)
- [Database backup and Restore](#database-backup-and-restore)

# Production enviroment set up

## Provison virtual machine (vm)
The first step in this project of a database migration from an on site systems to using Azure cloud computing services was to first set up a virtual machine. A virtual machine in cloud computing is a software-based emulation of a physical computer, allowing  operating systems to run on a server. Providing the end users with the flexibility to deploy and manage various computing environments within the cloud infrastructure. This why it is the correct choice for this project. To achieve this using Azure cloud computing resources is simple upon creation of an Azure cloud computing account and signing into the Azure portal. I simple search through the azure resources (create resource option on the portals homepage) and find the virtual machine option. 

Once selected all I had to do is select the resource group the vm is to be attached to and them select a logical name for the vm. Follwoing this i had to select a region where the vm would be hosted. Generally this should be the closest to my gepgraphcial location to minimsie latencies. Which for me is UK South option and therfore the one that was selected for this database migration project. After this I selected an image for the vm which for this proejct was the windows 11 pro option. Then I selected the size of image which for this database migration I selected the standard size option. Following on from this all that is required is the specification of user name and password, followed by the network ports which were set using RDP as I connected to virtual machine using a Ubuntu OS and the Remmima remote desktop client.

## Download and install SQL Server and Microsoft SQL Server Management Studio (SSMS)

Following the vm generation two essential softwares were required for use on the windows 11 OS to allow for database migration. These softwares are [SQL server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) and [SSMS](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16) These tools are essential for database management and migration using Azure cloud computing.

## Produce production database
The production database now can be created. This was achieved by using the downloaded [AdventureWorks2022.bak](https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms#download-backup-files) file and SSMS restore database functionality.


# Migrate to Azure SQL database

## Create Azure SQl database

The next step in Azure database migration was to generate a SQL database. To reiterate Azure services make this simple and intuitive. Using the Azure portal a Azure SQL database can be created by selecting the SQL database resource from the create a resource from the home portal. 

Through this process you have to generate a linked SQL server when doing this it is essential that SQL login are the choosen authetication method and firewall settings on the generated SQL databse are set as such that the vm IP address is added to the SQL server generated. This was achieved through Azure SQL Server set up menus.

## Install Azure data studio
In order to complete a database migration [Azure data studio](https://learn.microsoft.com/en-us/azure-data-studio/download-azure-data-studio?tabs=win-install%2Cwin-user-install%2Credhat-install%2Cwindows-uninstall%2Credhat-uninstall) must be downloaded and installed. By doing this a server connection can be created between azure data studio and the SQL Server hosted on the VM. In addition to connecting to the SQL server running on the Azure VM a connection must also be made to Azure SQL server database in the steps described [above](#create-azure-sql-database) generated.
In order to complete a database migration [Azure data studio](https://learn.microsoft.com/en-us/azure-data-studio/download-azure-data-studio?tabs=win-install%2Cwin-user-install%2Credhat-install%2Cwindows-uninstall%2Credhat-uninstall) must be downloaded and installed. By doing this a server connection can be created between azure data studio and the SQL Server hosted on the VM. In addition to connecting to the SQL server running on the Azure VM a connection must also be made to Azure SQL server database using the steps described [above](#create-azure-sql-database).


## Connect to Azure SQL Database
Azure Data Studio allows for connection to SQL Databases on local machines/vms as well as those running on Azure cloud services. This achieved by simple running the Azure data studio

## Schema migration 
With connections created using Azure data studio it was now possible to proceed with schema migration. To achieve this the  [SQL Server Schema Compare extension](https://learn.microsoft.com/en-us/azure-data-studio/extensions/schema-compare-extension) had to be downloaded through Azure data studios extension marketplace.

 Once succesfully installed schema comaprison/migration is a easy and right clicking on vm hosted database and selecting the Schema compare option. This create the schmea compare GUI with the vm database set as the source. Now the target database must choosen which of course is the Azure SQL database stored on the azure SQL Server. Folwing this it was simple as selecting the compare option on the GUI and then migrating the schema to the Azure SQL database.

 ## Data Migration
With correct schema migration the actual data can be migrated appropriately. In order to achieve this another Azure data studio Extension 


## Validate migration
With any migration a visual/programmtic assessment of the migration is neccesary. This is achieved by sanity checks such as comparing the number of tables in each database.


# Database backup and restore.

With any Database migration it is always useful to generate backup copies of the database/'s you are working with. The database can be backed to a .bak file using SSMS. This is simple achieved by selecting the Adventure works database stored on the Azure VM and right clicking and selecting the Back up option. Now uisng the Backup GUI dialog boxes a full restore and the to disk option are selected and using the backup folder default to store the .bak file. With these options succesfully specified Ok is then selected. SSMS makes you aware of of succesful backup of the database choosen.


## Restore database on development enviroment

When doing anything is software and tech devlopment/test envirmoments are incredible usefull for product development without disturning production enviroments.

As such the next step of the Database migration process engaged with here was to generate a development enviroment this is achieved by repeating tbe steps in [ Production enviroment set up](#production-enviroment-set-up)

# Microsoft Entra directory integration
The following step decription is about using the availible tooling of Microsoft Azure to increase the security around the databases we have migrated on the Azure SQL database services. Specifically Microsoft Entra ID.

## Create Azure SQL Microsoft entra Admin account



# Database migration diagram schema
![Azure Project Diagram](https://github.com/HPCurtis/azure-database-migration797/assets/136170998/7b809b53-335a-465c-894d-e08ef6be291e)

# Azure data base migration
The description of an implementaion of a database migration using the various tools and sotwares provided by the Microsoft [Azure cloud computing services](https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize?client_id=8e0e8db5-b713-4e91-98e6-470fed0aa4c2&response_type=code%20id_token&scope=openid%20profile&state=OpenIdConnect.AuthenticationProperties%3DjZEd0hq1c0oKq_PnKRqosFjsPt1YLhnhbcLnVWsmz_wuJhb4z6iRBjsenupkL0o2scLAQMHEbpfhlY_R23zsvheNvZWuKKWEFPRnC_kQDtWKc2PVaVIYexSeyWNrE-VNjaT2oEQQH_K2HtMQdX__-ySAInJpHBKHTM-ypIkDDVUtbQzuVng2lMq7wfhB7SeNJgbI0h93fLphQG5oXQtKS3LPvb7luxrEJw2yy2PIiPvI8AmpaV4IFYnSZOOyRJdrcB9XIBGDp4jJ5ZGfyxcSO9CPT786hzj8xOZLnXvNIgSSM2uPS2dksDPHyGAxjjFkX862Ys6dWwPIh_T7LXkHJiUgi9mo9lS8YrnoKKULgpJST9gMqmde2So0xZkaHuuNb_cpeHaARt0ZLyLy_vIh2jFzsYCjUUVALc-hiRI0-LrAJuqOu78xc9BSQwRt_9GEpS_96vJckfRnTwolCFHlDcAd-zoR177TUOOVYEcCZCY&response_mode=form_post&nonce=638407554444943112.NTBiNmM4MmYtNWFiMS00MzAwLWI2MTEtNjM5MDExMTI2NGFjZWY0ZTE0YmItMDAxMy00MjUzLTk5NTItY2RkZWZlZDg1N2Jl&redirect_uri=https%3A%2F%2Fsignup.azure.com%2Fapi%2Fuser%2Flogin&max_age=86400&post_logout_redirect_uri=https%3A%2F%2Fsignup.azure.com%2Fsignup%3Foffer%3Dms-azr-0044p%26appId%3D102%26ref%3D%26redirectURL%3Dhttps%3A%2F%2Fazure.microsoft.com%2Fget-started%2Fwelcome-to-azure%2F%26l%3Den-us%26srcurl%3Dhttps%3A%2F%2Fazure.microsoft.com%2Ffree&x-client-SKU=ID_NET472&x-client-ver=6.34.0.0)

Such tools provide end users ease and advanced tooling and is a majot point of the following description.

# Production enviroment set up

## Provison virtual machine (vm)
The first step in this project of database migration from on site systems to using Azure colud computing options was to first set up virtual machine. A virtual machine in cloud computing is a software-based emulation of a physical computer, allowing  operating systems to run on a server. pProviding the end users with the flexibility to deploy and manage various computing environments within the cloud infrastructure. This why it is the correct choice for this project. To achieve this using the Azure cloud computing resources is simple upon creation of an Azure cloud compting account and signing into the portal. End users must simple search throiugh the azure resources and find the virtual machine option. Once selected all i have to do is select the reosuce group the vm is to be attached to select a logical name for the vm select a region which should be the closest to my gepgraphcial location which is UK South Selct and image which for this proejct was windows 11 pro option. Then I selected the size of image which for this databse migration example i selected the standard option. Folwwing on from this all that is required is the specification of User name and password, follwed by the network ports which were set using RDP as i connected to virtual machine using a linux OS and the Remmima remote desktop client.

## Download and install SQL Server and Microsoft SQL Server Management Studio (SSMS)

Following vm generation two essential softwares are rewuired for use in the windows 11 OS to allow for database migration. These softwares are [SQL server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) and [SSMS](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16) Thesee tools are essetnail for database managment and migration using Azure cloud computing.

## Produce production database
The production database can now be created. This was achieved by using the downloaded [AdventureWorks2022.bak](https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms#download-backup-files) file and SSMS restore databse functionality.


# Migarete to Azure SQL database

## Create Azure SQl database

The next step in Azure database migration is to generate a SQL database. To reiterate Azure services make this simple and intuitive. Using the Azure portal a Azure SQL database can be created by selecting the SQL database resource from the create a resource from the home portal. Throught this process you have to generate a linked SQL server when doing this it is essential that SQL login credentials are selected and firewall restrictions are removed by adding the vm IP address.

## Install Azure data studio
In order to comple a database migration [Azure data studio](https://learn.microsoft.com/en-us/azure-data-studio/download-azure-data-studio?tabs=win-install%2Cwin-user-install%2Credhat-install%2Cwindows-uninstall%2Credhat-uninstall) must be dowlaoded and installed. By dowing this a server connection can be created between azure data studio and the SQL Server hosted on the VM. In addtion to connecting to the SQL server on Azure VM


![Azure Project Diagram](https://github.com/HPCurtis/azure-database-migration797/assets/136170998/7b809b53-335a-465c-894d-e08ef6be291e)

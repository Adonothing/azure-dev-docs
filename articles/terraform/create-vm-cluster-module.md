---
title: Configure an Azure VM cluster using Terraform
description: Learn how to use Terraform modules to create a Windows virtual machine cluster in Azure.
keywords: azure devops terraform vm virtual machine cluster module registry
ms.topic: how-to
ms.date: 08/07/2021
ms.custom: devx-track-terraform
---

# Configure an Azure VM cluster using Terraform

[!INCLUDE [Terraform abstract](./includes/abstract.md)]

This article shows example Terraform code for creating a VM cluster on Azure.

In this article, you learn how to:

> [!div class="checklist"]
> * Configure an Azure VM cluster

## 1. Configure your environment

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

[!INCLUDE [configure-terraform.md](includes/configure-terraform.md)]

## 2. Implement the code

1. Create a directory in which to test the sample Terraform code and make it the current directory.

1. Create a file named `main.tf` and insert the following code:

    ```hcl
    module "windowsservers" {
      source              = "Azure/compute/azurerm"
      resource_group_name = azurerm_resource_group.rg.name
      is_windows_image    = true
      vm_hostname         = "mywinvm"                         // Line can be removed if only one VM module per resource group
      admin_password      = "ComplxP@ssw0rd!"                 // See note following code about storing passwords in config files
      vm_os_simple        = "WindowsServer"
      public_ip_dns       = ["winsimplevmips"]                // Change to a unique name per data center region
      vnet_subnet_id      = module.network.vnet_subnets[0]
        
      depends_on = [azurerm_resource_group.rg]
    }
    
    module "network" {
      source              = "Azure/network/azurerm"
      resource_group_name = azurerm_resource_group.rg.name
      subnet_prefixes     = ["10.0.1.0/24"]
      subnet_names        = ["subnet1"]
    
      depends_on = [azurerm_resource_group.rg]
    }
    
    output "windows_vm_public_name" {
      value = module.windowsservers.public_ip_dns_name
    }
    
    output "vm_public_ip" {
      value = module.windowsservers.public_ip_address
    }
    
    output "vm_private_ips" {
      value = module.windowsservers.network_interface_private_ip
    }
    ```
    
    **Key points:**
    
    - In the preceding code example, the variable `admin_password` is assigned a literal value for the sake of simplicity. There are many ways in which to store sensitive data such as passwords. The decision as to how you want to protect your data comes down to individual choices involving your particular environment and comfort level exposing this data. As an example of the risk, storing a file like this in source control could potentially result in the password being seen by others. For more information on this subject, HashiCorp has documented various ways to [declare input variables](https://www.terraform.io/docs/configuration/variables.html) and techniques for [managing sensitive data (such as passwords)](https://www.terraform.io/docs/state/sensitive-data.html).
    
## 3. Initialize Terraform

[!INCLUDE [terraform-init.md](includes/terraform-init.md)]

## 4. Create a Terraform execution plan

[!INCLUDE [terraform-plan.md](includes/terraform-plan.md)]

## 5. Apply a Terraform execution plan

[!INCLUDE [terraform-apply-plan.md](includes/terraform-apply-plan.md)]

## Troubleshoot Terraform on Azure

[Troubleshoot common problems when using Terraform on Azure](troubleshoot.md)

## Next steps

> [!div class="nextstepaction"] 
> [Learn more about using Terraform in Azure](/azure/terraform)

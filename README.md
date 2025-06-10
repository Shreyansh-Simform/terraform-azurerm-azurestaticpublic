# Azure Static Website Terraform Module

This Terraform module creates an Azure Static Website using Azure Storage Account with static website hosting enabled.

## Description

This module provisions the following Azure resources:
- Azure Resource Group
- Azure Storage Account with a random suffix
- Static Website hosting configuration

## Features

- Creates a unique storage account name using a random string suffix
- Configures static website hosting on the storage account
- Supports custom index and error pages
- Flexible storage account configuration options

## Requirements

| Name | Version |
|------|---------|
| terraform | >= 1.0.0 |
| azurerm | ~> 2.0 |
| random | >= 3.0 |

## Providers

| Name | Version |
|------|---------|
| azurerm | ~> 2.0 |
| random | >= 3.0 |

## Usage

### Basic Usage

```hcl
module "azure_static_website" {
  source = "./terraform-azurerm-azurestaticpublic"
  
  location                          = "centralindia"
  resource_group_name               = "my-static-website-rg"
  storage_account_name              = "mystaticwebsite"
  storage_account_tier              = "Standard"
  storage_account_replication_type  = "LRS"
  storage_account_kind              = "StorageV2"
  static_website_index_document     = "index.html"
  static_website_error_404_document = "error.html"
}
```

### Complete Example

```hcl
module "azure_static_website" {
  source = "./terraform-azurerm-azurestaticpublic"
  
  # Location and Resource Group
  location            = "centralindia"
  resource_group_name = "my-static-website-rg"
  
  # Storage Account Configuration
  storage_account_name             = "mystaticwebsite"
  storage_account_tier             = "Standard"
  storage_account_replication_type = "LRS"
  storage_account_kind             = "StorageV2"
  
  # Static Website Configuration
  static_website_index_document     = "index.html"
  static_website_error_404_document = "error.html"
}

# Output the website URL
output "static_website_url" {
  value = module.azure_static_website.storage_account_name
}
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| location | The Azure Region in which all resources should be created | `string` | n/a | yes |
| resource_group_name | The name of the resource group | `string` | n/a | yes |
| storage_account_name | The name of the storage account (will have random suffix appended) | `string` | n/a | yes |
| storage_account_tier | Storage Account Tier | `string` | n/a | yes |
| storage_account_replication_type | Storage Account Replication Type | `string` | n/a | yes |
| storage_account_kind | Storage Account Kind | `string` | n/a | yes |
| static_website_index_document | Static website index document | `string` | n/a | yes |
| static_website_error_404_document | Static website error 404 document | `string` | n/a | yes |

### Input Variable Details

- **location**: Choose from available Azure regions (e.g., "centralindia", "eastus", "westeurope")
- **storage_account_tier**: Typically "Standard" or "Premium"
- **storage_account_replication_type**: Options include "LRS", "GRS", "RAGRS", "ZRS"
- **storage_account_kind**: Recommended "StorageV2" for static websites

## Outputs

| Name | Description |
|------|-------------|
| resource_group_id | The ID of the created resource group |
| resource_group_name | The name of the created resource group |
| resource_group_location | The location of the created resource group |
| storage_account_id | The ID of the created storage account |
| storage_account_name | The name of the created storage account (includes random suffix) |

## Post-Deployment Steps

After running `terraform apply`, you need to manually upload your website files:

1. Navigate to the Azure Portal
2. Go to your storage account
3. Find the **Static website** section under **Data management**
4. Upload your `index.html` and `error.html` files to the `$web` container
5. Your static website will be available at the provided endpoint URL

## Example terraform.tfvars

```hcl
location                          = "centralindia"
resource_group_name               = "myrg1"
storage_account_name              = "staticwebsite"
storage_account_tier              = "Standard"
storage_account_replication_type  = "LRS"
storage_account_kind              = "StorageV2"
static_website_index_document     = "index.html"
static_website_error_404_document = "error.html"
```

## Notes

- The storage account name will have a 6-character random string appended to ensure uniqueness
- Storage account names must be globally unique across Azure
- Static website hosting is automatically enabled on the storage account
- You need to manually upload your HTML files to the `$web` container after deployment

## License

This module is provided as-is for educational and development purposes.

## Contributing

Feel free to submit issues and enhancement requests!
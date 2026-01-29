---
name: opentofu-azure
description: Generate OpenTofu (Terraform) code for Azure infrastructure with current provider versions and best practices
allowed-tools: Task, Read, Grep, Bash, Edit, Write
user-invocable: true
argument-hint: [description of infrastructure to create]
---

# OpenTofu + Azure Infrastructure Skill

This skill defines rules for generating OpenTofu (Terraform) code for Azure infrastructure.

## Data Currency Rules

- **ALWAYS** use MCP (OpenTofu Registry) to verify current provider version and correct resource syntax
- **ALWAYS** use Agent for MCP searches - save context window
- Do NOT use deprecated resources (e.g., `azurerm_virtual_machine` is DEPRECATED, use `azurerm_linux_virtual_machine` or `azurerm_windows_virtual_machine`)

## Provider Configuration

```hcl
terraform {
  required_version = ">= 1.0"
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
      # ALWAYS verify version via MCP - use current stable version
    }
  }
}

provider "azurerm" {
  features {}
}
```

## Naming Convention

Format: `{project}-{resource_type}-{identifier}`

Examples:
- Resource Group: `demo-rg-main`
- Virtual Network: `demo-vnet-main`
- Subnet: `demo-snet-web`
- Network Security Group: `demo-nsg-web`
- Public IP: `demo-pip-web`
- Network Interface: `demo-nic-web`
- Virtual Machine: `demo-vm-web`

## Region

- Use `swedencentral` as default region

## Network Address Ranges

For demo/dev environments, use minimal network ranges:
- Virtual Network: `/24` (e.g., `10.0.0.0/24`)
- Subnet: `/27` or `/28` (e.g., `10.0.0.0/27` provides 32 addresses, enough for small demos)
- This provides sufficient addresses while being economical

## VM Specifications (for demo/dev)

- SKU: `Standard_B2s` (2 vCPU, 4 GB RAM)
- OS: Ubuntu LTS (preferably 22.04 or 24.04)
- Authentication: SSH key (not password)
- Disk: Standard SSD, minimum size

## File Structure

```
project/
├── main.tf           # Main resources
├── variables.tf      # Input variables
├── outputs.tf        # Output values
└── terraform.tfvars  # Variable values (NOT in git)
```

## Required Outputs

For VM with web server, always define:
- `public_ip` - public IP address
- `ssh_command` - SSH connection command
- `web_url` - HTTP URL for web server (use nip.io format: `http://{public_ip}.nip.io`)
- `web_url_https` - HTTPS URL for web server (use nip.io format: `https://{public_ip}.nip.io`)

## Network Security Group

- ALWAYS use `azurerm_network_interface_security_group_association` to link NSG with NIC
- Allow only necessary ports:
    - SSH (22) - open to all for demo purposes
    - HTTP (80) - for web server
    - HTTPS (443) - for web server

## Web Server Setup

For simple HTML page, use `custom_data` with cloud-init:

```yaml
#cloud-config
package_update: true
packages:
  - nginx
write_files:
  - path: /var/www/html/index.html
    content: |
      <!-- HTML content -->
runcmd:
  - systemctl enable nginx
  - systemctl start nginx
```

If additional files are needed (CSS, JS), add them via `write_files` as well.

## Checklist Before `tofu apply`

1. [ ] Provider version verified via MCP
2. [ ] Using current (not deprecated) resources
3. [ ] NSG association via `azurerm_network_interface_security_group_association`
4. [ ] SSH key, not password
5. [ ] Outputs defined (public_ip, ssh_command, web_url, web_url_https)
6. [ ] cloud-init for web server setup

## Workflow

When invoked:

1. Use Task agent with OpenTofu MCP to verify current azurerm provider version
2. Use Task agent with OpenTofu MCP to verify correct syntax for required resources
3. Generate files according to structure above
4. Follow naming convention
5. Include all required outputs
6. After generation, run `tofu init` and `tofu plan`
7. Analyze plan output:
   - Count resources to be created/changed/destroyed
   - Check for any errors or warnings
   - Verify outputs are defined
8. If plan is successful, use AskUserQuestion tool to ask if they want to proceed with `tofu apply` (show the number of resources to be created)

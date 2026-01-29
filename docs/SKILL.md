# Skill: OpenTofu + Azure Infrastructure

## Účel

Tento skill definuje pravidla pro generování OpenTofu (Terraform) kódu pro Azure infrastrukturu.

## Základní pravidla

### Aktuálnost dat

- **VŽDY** použij MCP (OpenTofu Registry) pro zjištění aktuální verze provideru a správné syntaxe resources
- **VŽDY** použij Agenta pro vyhledávání v MCP - šetři kontextové okno
- Nepoužívej zastaralé resources (např. `azurerm_virtual_machine` je DEPRECATED, použij `azurerm_linux_virtual_machine` nebo `azurerm_windows_virtual_machine`)

### Provider konfigurace

```hcl
terraform {
  required_version = ">= 1.0"
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
      # Verzi VŽDY ověř přes MCP - použij aktuální stable verzi
    }
  }
}

provider "azurerm" {
  features {}
}
```

### Naming convention

Formát: `{project}-{resource_type}-{identifier}`

Příklady:

- Resource Group: `demo-rg-main`
- Virtual Network: `demo-vnet-main`
- Subnet: `demo-snet-web`
- Network Security Group: `demo-nsg-web`
- Public IP: `demo-pip-web`
- Network Interface: `demo-nic-web`
- Virtual Machine: `demo-vm-web`

### Region

- Používej `swedencentral` jako výchozí region

### VM specifikace (pro demo/dev)

- SKU: `Standard_B2s` (2 vCPU, 4 GB RAM)
- OS: Ubuntu LTS (preferovaně 22.04 nebo 24.04)
- Authentication: SSH klíč (ne heslo)
- Disk: Standard SSD, minimální velikost

### Struktura souborů

```
project/
├── main.tf           # Hlavní resources
├── variables.tf      # Input proměnné
├── outputs.tf        # Output hodnoty
└── terraform.tfvars  # Hodnoty proměnných (NENÍ v gitu)
```

### Povinné outputs

Pro VM s web serverem vždy definuj:

- `public_ip` - veřejná IP adresa
- `ssh_command` - příkaz pro SSH připojení

### Network Security Group

- VŽDY použij `azurerm_network_interface_security_group_association` pro propojení NSG s NIC
- Povol pouze nezbytné porty:
    - SSH (22) - pro demo účely otevřeno všem
    - HTTP (80) - pro web server
    - HTTPS (443) - pro web server

### Web server setup

Pro jednoduchou HTML stránku použij `custom_data` s cloud-init:

```yaml
#cloud-config
package_update: true
packages:
  - nginx
write_files:
  - path: /var/www/html/index.html
    content: |
      <!-- HTML obsah -->
runcmd:
  - systemctl enable nginx
  - systemctl start nginx
```

Pokud budou potřeba další soubory (CSS, JS), přidej je taktéž přes `write_files`.

## Checklist před `tofu apply`

1. [ ] Provider verze ověřena přes MCP
2. [ ] Použity aktuální (ne deprecated) resources
3. [ ] NSG asociace přes `azurerm_network_interface_security_group_association`
4. [ ] SSH klíč, ne heslo
5. [ ] Outputs definovány (public_ip, ssh_command)
6. [ ] cloud-init pro web server setup

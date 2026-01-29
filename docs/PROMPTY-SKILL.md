# Prompty pro vytvoření SKILL souborů

Tyto prompty můžeš použít k popisu, jak vznikly SKILL soubory.

---

## Prompt pro SKILL.md (OpenTofu + Azure)

```
Vytvoř SKILL.md soubor pro práci s OpenTofu a Azure.

Požadavky na projekt:
- Linux VM s veřejnou IP
- Web server (nginx)
- Region: swedencentral
- VM: levné SKU pro demo (2 CPU, 4 GB RAM)
- Pro SSH nepoužij heslo, ale SSH klíč a ten si vygeneruj lokálně a ulož do @.ssh/ (nastav správná práva)

Skill má obsahovat:
1. Pravidlo: VŽDY ověřit aktuální verzi provideru přes MCP
2. Pravidlo: VŽDY použít AGENTA pro MCP dotazy (šetřit kontext)
3. Pravidlo: Nepoužívat deprecated resources
4. Naming convention (jednoduchá, pro demo)
5. Struktura souborů (main.tf, variables.tf, outputs.tf)
6. Povinné outputs (public_ip, ssh_command)
7. NSG pravidla (SSH 22 otevřeno všem, HTTP 80 a 443 otevřeno všem)
8. Cloud-init pro nginx setup
9. Checklist před tofu apply
```

---

## Prompt pro SKILL-TEST.md (Playwright testování)

```
Vytvoř SKILL-TEST.md pro testování nasazené VM přes Playwright MCP.

Postup má být:
1. Získat IP z tofu output
2. Curl loop - počkat na nginx (max 2 minuty)
3. Přes AGENTA a Playwright MCP otevřít stránku
4. Udělat screenshot a uložit do @outputs
5. Analyzovat - ověřit QR kód a URL text

Důležité:
- Všechno dělá AGENT
- Žádný vlastní kód - použít MCP
- Screenshot musí být uložen pro prezentaci
```

---

## Poznámky k prezentaci

Když budeš popisovat vznik SKILL souborů:

> "Tenhle SKILL.md jsem si připravil předem. Vznikl promptem, kde jsem Claudovi popsal, co potřebuju - region, velikost VM, že chci aktuální verze z MCP. Claude vygeneroval pravidla a checklist."

Nemusíš ukazovat live generování - stačí popsat postup a ukázat hotový soubor.

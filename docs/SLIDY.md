# Slidy - Výplňový obsah

---

## Slide 1: autowt (Git Worktrees)

**Co to je:**

- Automatizace git worktrees pro práci s feature větvemi
- Každá větev = vlastní adresář, žádné stashing/switching

**Proč to používám:**

- Claude Code může pracovat v izolovaném prostředí
- Paralelní práce na více větvích bez konfliktů
- Čistý stav pro každý experiment

**Příkaz:**

```bash
autowt feature/nova-vm
# → vytvoří ../repo-feature-nova-vm/
```

---

## Slide 2: Claude v Dockeru

**Problém:**

- `--dangerously-skip-permissions` = Claude může spouštět cokoliv
- Na hostitelském systému = riziko

**Řešení:**

- Claude běží v Docker kontejneru
- Kontejner má přístup jen k pracovnímu adresáři
- AZ CLI token předán jako env variable

**Setup:**

```bash
docker run -it \
  -v $(pwd):/workspace \
  -e AZURE_CONFIG_DIR=/workspace/.azure \
  claude-code --dangerously-skip-permissions
```

---

## Slide 3: Service Principal - minimální práva

**Princip nejmenších oprávnění:**

- SP má práva JEN na resource group pro demo
- Contributor na RG, nic víc
- Token expiruje po prezentaci

**Co může:**

- Vytvořit VM, VNet, NSG, Public IP
- Smazat co vytvořil

**Co nemůže:**

- Přistupovat k jiným RG
- Měnit subscription-level nastavení
- Vytvářet nové SP nebo role

---

## Slide 4: Tipy na nástroje

### Ralph

- Pro vyšperkování výstupu
- Iterativní vylepšování s lidským feedbackem
- Když chceš "perfektní" výsledek

### Automaker

- Komplexní, vícekrokové úlohy
- Automatická dekompozice problému
- Když je zadání složité a nejasné

### Moltbot

- Důraz na bezpečnost
- Izolované prostředí pro běh kódu
- Audit trail všech akcí
- https://www.shodan.io/search?query=clawdbot-gw

---

## Slide 5: Shrnutí - Na první dobrou vs S přípravou

| Aspekt   | Na první dobrou       | S přípravou         |
|----------|-----------------------|---------------------|
| Kontext  | Žádný                 | SKILL.md, MCP       |
| Data     | Trénovací (zastaralá) | Aktuální z registrů |
| Výsledek | Funguje, ale...       | Funguje správně     |
| Iterace  | Mnoho                 | Minimum             |
| Čas      | Zdánlivě rychle       | Reálně rychleji     |

**Takeaway:** Investice do přípravy se vrátí.

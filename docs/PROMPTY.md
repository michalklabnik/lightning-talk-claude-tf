# Prompty pro Lightning Talk

## Část 1: "Na první dobrou"

### Varianta A: Vágní prompt

```text
Vytvoř Terraform pro Azure VM s veřejnou IP a webovým serverem.
```

**Proč je to problém:**

- Žádná specifikace verze provideru
- Žádný naming convention
- Žádný region
- Žádná velikost VM
- Claude použije to, co má v trénovacích datech (často zastaralé)

---

### Varianta B: "Špatný" prompt (trochu lepší, ale stále nedostatečný)

```text
Vytvoř Terraform kód pro Azure, který:
- Vytvoří Linux VM
- VM bude mít veřejnou IP adresu
- Na VM poběží nginx s jednoduchou HTML stránkou
- Použij Ubuntu

Potřebuju všechny potřebné soubory.
```

**Proč je to stále problém:**

- Pořád chybí verze provideru → Claude může použít zastaralou
- Pořád chybí naming convention → generický output
- Chybí specifikace, jak se připojit (SSH klíč vs heslo)
- Chybí NSG pravidla
- Claude nemá aktuální dokumentaci → deprecated resources

---

## Část 2: "S přípravou" (hlavní prompt)

### Kontext před promptem

**Předpoklady:**

1. Aktivní Skill soubor (`SKILL.md`) v projektu
2. MCP servery nakonfigurované:
    - `context7` - pro obecnou dokumentaci
    - `opentofu-registry` - pro aktuální verze providerů a resources
3. Claude běží v Dockeru s `--dangerously-skip-permissions`
4. AZ CLI přihlášený přes Service Principal

---

### Hlavní prompt

```text
Použij skill opentofu-azure.

Cíl:
- Linux VM s veřejnou IP adresou
- Na VM poběží nginx web server
- Web server zobrazí stránku z přiloženého souboru @docs/html/index.html
- Stránka obsahuje i obrázek @docs/html/qr-code.png - ten je potřeba také umístit na VM
- Nastav můj SSH klíč @docs/assets/id_ed25519-cloudfield.cz.pub a pro sebe si vygeneruj vlastní (ulož do @.ssh/)
- Použij existující resource group claude-sandbox
```

**Proč tohle funguje:**

- Odkazuje na Skill soubor s pravidly
- Explicitně říká "použij AGENTA" → šetří kontext
- Explicitně říká "použij MCP" → aktuální data
- Specifikuje konkrétní soubory (index.html, qr-code.png)
- Definuje postup krok za krokem
- Říká, co má udělat po vygenerování

---

## Alternativní prompty

### Pro Playwright test (přes MCP)

```text
/test-deployment
```

---

## Tipy pro prezentaci

### Když Claude "přemýšlí" dlouho

- Přepni na slide o autowt / Docker setupu
- Mluv o bezpečnosti SP s minimálními právy
- Zmíň další nástroje (Ralph, Automaker, Moltbot)

### Když něco nefunguje

- "Pro úsporu času použiju připravený kód"
- Ukaž diff mezi vygenerovaným a opraveným
- Vysvětli, co bylo špatně

### Pro Q&A

- Měj otevřený SKILL.md - lidi se budou ptát na strukturu
- Měj připravenou odpověď na "kolik to stojí" (B2s ~ €30/měsíc, ale běží hodinu)

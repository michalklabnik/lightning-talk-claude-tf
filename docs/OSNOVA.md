# Lightning Talk: Claude Code CLI + OpenTofu + Azure

## Metadata

- **Délka:** 45 minut
- **Formát:** Live coding + slidy jako výplň
- **Nástroje:** Claude Code CLI, OpenTofu, Azure, Playwright

---

## Část 1: "Na první dobrou" (~15 min)

| Čas   | Obsah                                         | Typ     |
|-------|-----------------------------------------------|---------|
| 2 min | Intro - co budeme dělat, jaký je cíl          | Mluvené |
| 5 min | Jednoduchý prompt → Claude generuje TF        | Live    |
| 5 min | `tofu plan/apply` → problémy, errory, iterace | Live    |
| 3 min | Shrnutí problémů                              | Mluvené |

### Cíl části

- Ukázat, že "naivní" přístup vede k nejistým výseldkům a problémům
- Demonstrovat typické chyby (deprecated resources, chybějící asociace, ...)
- Vytvořit kontrast pro druhou část

---

## Část 2: "S přípravou" (~30 min)

| Čas   | Obsah                                    | Typ     | Poznámka             |
|-------|------------------------------------------|---------|----------------------|
| 3 min | Co uděláme jinak - přehled přístupu      | Mluvené | MCP, Skills, kontext |
| 3 min | autowt, Docker setup, SP bezpečnost      | Slide   | Výplň při čekání     |
| 5 min | Skill soubor - ukázka + popis jak vznikl | Ukázka  | Předpřipravený       |
| 8 min | Prompt → Claude generuje TF              | Live    | S MCP + Skills       |
| 3 min | Ralph, Automaker, Moltbot                | Slide   | Tipy na nástroje     |
| 5 min | `tofu apply` → funguje                   | Live    | Deploy               |
| 3 min | Playwright → web + QR kód                | Live    | Důkaz                |

### Cíl části

- Ukázat, jak kontext a nástroje zlepšují výsledek
- Demonstrovat praktické tipy (autowt, Docker, bezpečnost)
- Funkční deploy jako důkaz

---

## Příprava před prezentací

### Předpřipravené materiály

- [x] Skill soubor (`SKILL.md`)
- [x] Prompty (naivní + s přípravou)
- [ ] Slidy (autowt, Docker, Ralph/Automaker/Moltbot)
- [ ] Fallback TF kód (funkční, v jiné větvi)
- [x] HTML stránka s QR kódem na prezentaci

### Technické nastavení

- [x] Docker s Claude Code CLI (`--dangerously-skip-permissions`)
- [x] Azure SP s minimálními právy
- [x] MCP servery nakonfigurované (context7, OpenTofu Registry)
- [x] autowt nainstalovaný a funkční
- [x] Git repo s větvemi připravenými

### Fallback plán

1. Pokud Claude generuje příliš dlouho → přepnout na předpřipravený kód
2. Pokud deploy selže → ukázat předpřipravený funkční stav
3. Pokud Playwright selže → ruční refresh stránky jako důkaz

---

## Klíčové body k zmínění

### Část 1

- LLM mají zastaralá trénovací data
- Bez kontextu = generický výstup
- Iterace stojí čas

### Část 2

- MCP = aktuální data z registrů
- Skills = konzistentní pravidla
- Agenti = šetření kontextového okna
- Docker = bezpečné `--dangerously-skip-permissions`
- autowt = čistá práce s větvemi

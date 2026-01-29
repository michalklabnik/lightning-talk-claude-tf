# Skill: Testování nasazené stránky přes Playwright MCP

## Účel

Instrukce pro ověření, že nasazená VM s web serverem funguje správně.

## Kdy použít

Po úspěšném `tofu apply`, když chceš ověřit funkčnost.

## Postup

### 1. Získej IP adresu

```bash
tofu output -raw public_ip
```

### 2. Počkej na nginx

Před testováním ověř, že nginx běží. Použij curl v loopu:

```bash
for i in {1..24}; do
  curl -s -o /dev/null -w "%{http_code}" --max-time 5 http://IP && break
  echo "Pokus $i - čekám..."
  sleep 5
done
```

### 3. Otestuj přes Playwright MCP

Pomocí AGENTA a Playwright MCP:

- Otevři stránku na http://IP
- Udělej screenshot celé stránky
- Ulož screenshot do /outputs

### 4. Analyzuj výsledek

Po uložení screenshotu zkontroluj:

- Je vidět QR kód?
- Je vidět URL text "klabnik.link/talk-claude-tf"?

## Očekávaný výsledek

- Screenshot uložený v /outputs
- Potvrzení, že stránka obsahuje QR kód a URL

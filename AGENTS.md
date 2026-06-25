# OpenCode Agent Config

Raccolta di skill e agenti per OpenCode e altri agenti compatibili. Le skill sono organizzate per ambiti di competenza sotto `skills/`.

## Struttura

```
skills/
  prodotto/        # Competenze di prodotto
  foundation/
    plan/          # Pianificazione e architettura
    coding/        # Implementazione e qualità
agents/            # Definizioni agente .md
```

## Scrivere Skill

- Ogni skill è una directory sotto `skills/` con `SKILL.md` YAML frontmatter (`name`, `description`)
- SKILL.md sotto 500 righe; usa `references/` per documentazione approfondita
- Tono imperativo, spiega il *perché* oltre al *cosa*
- Nome directory in kebab-case, corrisponde al campo `name` in frontmatter

## Scrivere Agenti

- Gli agenti sono file `.md` in `agents/` con frontmatter YAML
- Frontmatter richiede: `name`, `description`, `skills` (lista di skill name)
- Il corpo contiene le istruzioni specifiche dell'agente

## Validazione

Prima del commit:
- `name` e `description` presenti in ogni SKILL.md
- `name` corrisponde al nome della directory
- Ogni SKILL.md elencato in `skills/` è referenziato da almeno un agente

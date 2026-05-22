# Skill: Stanadardize Branch

## Purpose
Normalize and standardize branch input.
into format {area}-{number}

---

## Supported Input Formats

- domain-{area}-{number}
- {area}-{number}
- {area} {number}
- {area}{number}

---

## Domain Prefix Handling

- If input is in format domain-{area}-{number}
  Automatically convert to {area}-{number}

- If input matches pattern {area}{number}
  Automatically convert to {area}-{number}

---

## Examples

energy120 → energy-120 ✅ 
powertrain-999 → powertrain-999 ✅  

---

## Output

standardize branch in format {area}-{number}
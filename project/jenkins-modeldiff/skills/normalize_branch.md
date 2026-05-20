# Skill: Normalize Branch

## Purpose
Normalize and standardize branch input.

---

## Supported Input Formats

- domain-{area}-{number}
- {area}-{number}
- {area} {number}
- {area}{number}

---

## Normalization Rules

1. Fix typos:
   - domian → domain
   - doamin → domain
   - damain → domain
   - doman → domain

2. Normalize separators:
   - `_`, `.`, space → `-`

3. Convert to lowercase

---

## Domain Prefix Handling

- If input is in format:
  {area}-{number}

- Automatically convert to:
  domain-{area}-{number}

- If input matches pattern:
  {area}{number}

- Split into:
  {area}-{number}

---

## Examples

energy120 → energy-120 ✅ 
powertrain-999 → domain-powertrain-999 ✅  

---

## Output

normalized_branch
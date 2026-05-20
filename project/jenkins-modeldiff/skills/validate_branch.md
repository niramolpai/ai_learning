# Skill: Validate Branch

## Purpose
Validate branch format and structure.

---

## Required Pattern

domain-{area}-{number}

---

## Rules

- starts with "domain-"
- has exactly 3 parts
- area must be letters only
- number must be digits only

---

## Area Validation

- Valid areas should be read from:
  scripts/domain.txt

- If available, only values in domain.txt are considered valid

- If domain.txt is not available:
  - skip strict validation

---

## Output

- valid = true/false
- reason if invalid

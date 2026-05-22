# Skill: Validate Branch

## Purpose
Validate branch format and structure.

---

## Required Pattern

{area}-{number}

---

## Rules

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

- If valid, continue to create pr link
- if invalid, Stop processing and provide reason

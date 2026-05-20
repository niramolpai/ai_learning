# Skill: Suggest Area

## Purpose
Suggest the closest matching area when the provided `{area}` is invalid.

---

## Input

- invalid `{area}`
- list of valid areas from `scripts/domain.txt`

---

## Behavior

- Compare `{area}` with available values
- Identify the closest match based on similarity

---

## Output

- suggested_area (if found)
- null (if no close match)

---

## Examples

energi → energy ✅  
powrtrain → powertrain ✅  
xyz → null ❌
``
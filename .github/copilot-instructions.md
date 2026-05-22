## Jenkins Execution

The system must trigger the Jenkins job after all conditions are satisfied.

---

### Execution Conditions

Trigger ONLY IF:

1. intent = ModelDiff_PR
2. a `{area}` and `{number}` validation has PASSED
3. PR URL is fully constructed and valid
4. NO confirmation is required if all validations pass

---

### Execution Action

Trigger Jenkins job by calling MCP tool `jenkins` with:

Job Name:
- ModelDiff_PR

Parameters:
- PR = <full URL>
- email = <resolved email if available>

Execution:

trigger_build(
  job="ModelDiff_PR",
  parameters={
    "PR": "https://cc.net/swh/ddad-domains-energy/pull/102",
    "email": "user@test.com"
  }
)

---

### Email Resolution Rules

The system must determine the email parameter as follows:

1. If the user input contains an email:
   - Use the provided email

2. If no email is provided in the user input:
   - Look for a default email in:
     `scripts/email.txt`
   - Use the email from the file

3. If no email is found in both input and file:
   - Execute without email

---

### Email Priority

1. User-provided email (highest priority)
2. Email from `scripts/email.txt`
3. No email (fallback)

---

## Branch Input Flexibility

Users may provide input in multiple formats:

- domain-energy-101
- energy-101
- energy 101
- energy101

---

## Branch Input Standardize

All inputs MUST be standardized to:

{area}-{number}

---

## Branch Input Validation

- Input MUST contain exactly one `{area}` and one `{number}`
- If no valid pattern is detected:
  - The system MUST respond with an error
  - Provide examples of valid inputs
  - STOP processing immediately

---

### Example (Invalid Input)

- "modeldiff energy" → missing number
- "modeldiff hello world" → no valid pattern

---

### Number Validation

- The `{number}` must be numeric only
- If `{number}` is not numeric:
  - The system MUST respond with an error
  - STOP processing immediately
- If `{number}` is missing:
  - The system MUST respond with an error
  - The response MUST indicate that a number is required
  - STOP processing immediately
- If multiple numeric values are detected:
  - The system MUST respond with an error
  - Ask the user to provide only one number
  - STOP processing immediately

---

## Domain Area Validation

The `{area}` component must be validated before execution.

---

### Domain Area Source

- Allowed areas are defined in:
  `scripts/domain.txt`
- The file must contain:
  - one value per line
  - lowercase values

Example:

energy  
powertrain  
driving  

---

### Domain Area Validation Rules

1. If `scripts/domain.txt` exists:
   - If `{area}` matches a value → continue to create PR
   - If `{area}` does NOT match:
    - Call `suggest_area` skill
      - If a HIGH-confidence match exists:
        - Automatically replace `{area}` with the suggested value
        - Log correction internally
      - If a low-confidence or multiple matches exist:
        - Ask for confirmation:
          "Did you mean `<suggested_area>`?"
        - STOP processing
      - If no match exists:
        - Respond with error
        - Include valid values
        - STOP processing
2. If `scripts/domain.txt` does NOT exist:
   - Skip strict validation

---

### Validation Order

- Validation MUST occur AFTER normalization
- Validation MUST occur BEFORE Jenkins execution

---

### Notes

- email parameter is optional
- DO NOT include email if not available
- MUST use exact parameter names
